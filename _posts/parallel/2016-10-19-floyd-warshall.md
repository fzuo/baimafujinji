---
layout: post
title: Floyd-Warshall算法及其并行化
category: 多核与并发
tags: MPI
keywords: MPI,并发编程, Floyd算法
---

> Floyd-Warshall算法（或称Floyd算法）是用于寻找加权图中非固定起止点间最短路径的经典算法，它是基于动态规划思想设计的。当前我们所认识的Floyd算法之形式由计算机科学家（同时也是图灵奖得主） Robert Floyd 于 1962 年提出并发表。但在此之前，Bernard Roy（1959）和 Stephen Warshall（1962）也分别独立地提出了类似的算法。

### 串行实现

鉴于Floyd算法非常有名，解释其基本原理的资料非常多，此处并不打算对Floyd算法本身做过多的赘述。如果你对Floyd算法本身还不甚了解，可以参考《算法之美——隐匿在数据结构背后的原理（C++版）》一书中的第8章第4节之介绍。

但是为了作为并行实现的对照版本，我们还是先给出一个用C++串行实现的Floyd算法。对于下面程序，我们做如下约定：

- 图是有向的； 

- 初始化时如果两个节点之间没有边直接可达，则赋值为-1（NOT_CONNECTED）；

- 节点的标记从1开始，即第1个节点，第2个节点，等等，但没有第0个节点。

下面给出示例代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>

using namespace std;

#define MAX 10
#define NOT_CONNECTED -1

int distance[MAX][MAX];

//number of nodes
int nodesCount;

//initialize all distances to 
void Initialize(){
    memset(distance, NOT_CONNECTED , sizeof(distance));

    for (int i=0;i<MAX;++i)
        distance[i][i]=0;
}

int main(){

    Initialize();

    //get the nodes count
    scanf("%d", &nodesCount);

    //edges count
    int m;
    scanf("%d", &m);

    while(m--){
        //nodes - let the indexation begin from 1
        int a, b;

        //edge weight
        int c;

        scanf("%d-%d-%d", &a, &c, &b);
        distance[a][b]=c;
    }

    //Floyd-Warshall
    for (int k=1;k<=nodesCount;++k){
        for (int i=1;i<=nodesCount;++i){
            if (distance[i][k]!=NOT_CONNECTED){
                for (int j=1;j<=nodesCount;++j){
                    if (distance[k][j]!=NOT_CONNECTED && (distance[i][j]==NOT_CONNECTED || distance[i][k]+distance[k][j]<distance[i][j])){
                        distance[i][j]=distance[i][k]+distance[k][j];
                    }
                }
            }
        }
    }

    for (int i = 1; i <= nodesCount; ++i)
    {
        for (int j = 1; j <= nodesCount; ++j)
        {
            printf("%d ", distance[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

上述代码读入一个用来表示有向加权图的文件，文件中第一行表示节点数，第二行表示边数，之后每一行表示一条加权表，例如graph.txt文件内容如下

```
4
5
1-1-2
1-10-4
2-2-3
2-3-4
3-1-4
```
其中1-1-2表示从节点1到节点2之间的边的权重为1。 

另外两个可以用于测试的图文件：graph2.txt

```
5
9
1-5-2
2-2-3
1-3-3
5-1-1
4-1-5
1-2-4
4-4-3
3-7-5
2-3-5
```
graph3.txt

```
5
7
1-13-5
1-6-4
5-2-2
1-6-2
2-3-3
4-1-3
4-5-5
```

执行我们的程序可得如下之结果：

```
$ g++-5 Floyd_s.cpp -o a.out
$ ./a.out<graph3.txt
0 6 7 6 11 
-1 0 3 -1 -1 
-1 -1 0 -1 -1 
-1 7 1 0 5 
-1 2 5 -1 0 
$ ./a.out<graph2.txt
0 5 3 2 3 
4 0 2 6 3 
8 13 0 10 7 
2 7 4 0 1 
1 6 4 3 0 
$ ./a.out<graph.txt
0 1 3 4 
-1 0 2 3 
-1 -1 0 1 
-1 -1 -1 0 
```

-----------------------------

### 并行实现

现在来讨论并行实现的思路。基本想法是把一个大矩阵，按行进行划分，每个处理器（或计算节点，注意是分布式Supercomputer上的节点，不是图中的节点）分别负责矩阵中的几行，例如我们的矩阵大小是16×16，准备在四个处理器上并行计算，那么就像下图一样把整个矩阵按行分为四个小矩阵：A、B、C、D。然后每个处理分别负责其中之一。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/floyd.png" width="420">
</p>

下面是我在C++下实现的基于MPI的并行Floyd算法。

```cpp
//Author: baimafujinji

#include <cstdio>
#include <cstdlib>
#include <cstring>
#include "mpi.h"

using namespace std;

#define MAX 10
#define NOT_CONNECTED -1

int distances[MAX][MAX];
int result[MAX][MAX];
//number of nodes
int nodesCount;

//initialize all distances to 
void Initialize(){

    memset(distances, NOT_CONNECTED , sizeof(distances));
    memset(result, NOT_CONNECTED , sizeof(result));

    for (int i=0;i<MAX;++i)
        distances[i][i]=0;
}

int cmp ( const void *a , const void *b ){
    return *(int *)a - *(int *)b;
}

int main(int argc, char *argv[]){

    Initialize();

    //get the nodes count
    scanf("%d", &nodesCount);

    //edges count
    int m;
    scanf("%d", &m);

    while(m--){
        //nodes - let the indexation begin from 1
        int a, b;

        //edge weight
        int c;

        scanf("%d-%d-%d", &a, &c, &b);
        distances[a][b]=c;
    }

    int size, rank;

    MPI_Init(&argc,&argv);

    MPI_Datatype rtype;

    MPI_Comm_size(MPI_COMM_WORLD, &size);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    int slice = (nodesCount)/size;

    MPI_Bcast(distances, MAX*MAX, MPI_INT, 0, MPI_COMM_WORLD);
    MPI_Bcast(&nodesCount, 1, MPI_INT, 0, MPI_COMM_WORLD);
    MPI_Bcast(&slice, 1, MPI_INT, 0, MPI_COMM_WORLD);

    //Floyd-Warshall
    int sent=1;

    for (int k=1;k<=nodesCount;++k){

        int th = 1;
        for(; th <= size; th++)
        {
            if(1+slice*(th-1) <= k && k <= slice*th)
                sent = th;
        }
        if(1+slice*(th-1) <= k && k <= nodesCount )
            sent = size;

        MPI_Bcast(&distances[k], nodesCount+1, MPI_INT, sent-1, MPI_COMM_WORLD);

        if(rank != size-1){

            for (int i=1+slice*(rank);i<=slice*(rank+1);++i){  
                if (distances[i][k]!=NOT_CONNECTED){
                    for (int j=1;j<=nodesCount;++j){
                        if (distances[k][j]!=NOT_CONNECTED && (distances[i][j]==NOT_CONNECTED 
                            || distances[i][k]+distances[k][j]<distances[i][j])){
                            distances[i][j]=distances[i][k] + distances[k][j];
                        }
                    }
                }
            }
        }

        else{
            for (int i=1+slice*rank;i<=nodesCount;++i){
                if (distances[i][k]!=NOT_CONNECTED){
                    for (int j=1;j<=nodesCount;++j){
                        if (distances[k][j]!=NOT_CONNECTED && (distances[i][j]==NOT_CONNECTED 
                            || distances[i][k]+distances[k][j]<distances[i][j])){
                            distances[i][j]=distances[i][k]+distances[k][j];
                        }
                    }
                }
            }
        }
    }


    for (int k=1;k<=nodesCount;++k){

        int th = 1;
        for(; th <= size; th++)
        {
            if(1+slice*(th-1) <= k && k <= slice*th)
                sent = th;
        }
        if(1+slice*(th-1) <= k && k <= nodesCount )
            sent = size;

        MPI_Bcast(&distances[k], nodesCount+1, MPI_INT, sent-1, MPI_COMM_WORLD);
    }

    MPI_Reduce(distances, result, MAX*MAX, MPI_INT, MPI_MIN, 0, MPI_COMM_WORLD);

    if(rank==0)
    {
        for(int i = 1; i <= nodesCount; i++)
        {
            for(int j = 1; j <= nodesCount; j++)
            {        
                printf("%d ", result[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    /* Shut down MPI */
    MPI_Finalize();

    return 0;
}
```

完成编码后，我们来验证一下上面程序的计算结果是否和串行实现的版本所得一致。

```
$ mpirun -n 3 ./a.out<graph3.txt
0 6 7 6 11 
-1 0 3 -1 -1 
-1 -1 0 -1 -1 
-1 7 1 0 5 
-1 2 5 -1 0 

$ mpirun -n 3 ./a.out<graph2.txt
0 5 3 2 3 
4 0 2 6 3 
8 13 0 10 7 
2 7 4 0 1 
1 6 4 3 0 

$ mpirun -n 3 ./a.out<graph.txt
0 1 3 4 
-1 0 2 3 
-1 -1 0 1 
-1 -1 -1 0 
```

可见我们的并行程序输出了预期的结果。

<span style="color:blue">**（本文完）**</span>
