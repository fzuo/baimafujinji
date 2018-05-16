---
layout: post
title: MPI中的Bcast, Reduce和Gather使用
category: 多核与并发
tags: MPI
keywords: Bcast,Reduce,Gather
---

### MPI_Gather

MPI中的Gather用于从不同节点中收集一个等长的序列（数组）到一个指定的节点中（一个同类型的数组中）。 

Instead of spreading elements from one process to many processes, MPI_Gather takes elements from many processes and gathers them to one single process. MPI_Gather的函数原型如下：

```
MPI_Gather(
    void* send_data,
    int send_count,
    MPI_Datatype send_datatype,
    void* recv_data,
    int recv_count,
    MPI_Datatype recv_datatype,
    int root,
    MPI_Comm communicator)
```

下面来看一个实例程序：

```cpp
//author: baimafujinji
#include <iostream>
#include <cstdlib>
#include "mpi.h"

#define N 4

using namespace std;

int main(int argc, char * argv[]) {

    int size, rank;
    MPI_Init(&argc,&argv);
    MPI_Datatype rtype;
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    double *collection;

    collection = (double*)malloc(sizeof(double)*size*N);

    double path_pack[N] = {1.0,1.0,1.0,1.0};

    for(int i = 0; i < N; i++)
        path_pack[i] += (double)rank;

    if(rank == 0){
        for (int j = 0; j < N; j++)
        {
            cout<<path_pack[j]<<" ";
        }
        cout<<endl;
    }

    if(rank == 1){
        for (int j = 0; j < N; j++)
        {
            cout<<path_pack[j]<<" ";
        }
        cout<<endl;
    }

    if(rank == 2){
    for (int j = 0; j < N; j++)
        {
            cout<<path_pack[j]<<" ";
        }
        cout<<endl;
    }

    MPI_Type_contiguous( N, MPI_DOUBLE, &rtype );
    MPI_Type_commit( &rtype );

    MPI_Gather(path_pack, N, MPI_DOUBLE, collection, 1, rtype, 0, MPI_COMM_WORLD);

    MPI_Barrier(MPI_COMM_WORLD);
    if(rank == 0){
        for(int i = 0; i<size; i++)
        {
            for (int j = 0; j < N; j++)
            {
                cout<<collection[i*N+j]<<" ";
            }
            cout<<endl;
        }
    }

    free(collection);

    MPI_Finalize();
}
```

上述代码的执行结果如下：

```
$ mpirun -n 3 ./a.out
2 2 2 2 
1 1 1 1 
3 3 3 3 
1 1 1 1 
2 2 2 2 
3 3 3 3
```

注意上述结果中由于三个节点是并行执行的，所以程序输出的头三行内容顺序是可变的，后三行这是按顺序统一收回后输出的，内容是不会变动的。需要注意的是，Gather一个2D array目前在MPI中还不能做到，也就是说如果上述d代码中的collection变成int**，这程序是会执行异常的，所以只能采用上面的方法将其转换为1D array再行处理。

----------------------------

### MPI_Bcast与MPI_Reduce

下面这个例子演示了MPI中Bcast和Reduce的使用。更具体地，我们不仅广播一个矩阵到所有有节点，我们还广播矩阵中的某一行，最后我们Reduce一个矩阵。

```cpp
//author: baimafujinji
#include <cstdio>
#include <cstring>
#include "mpi.h"

#define MAX 4

int matrix[MAX][MAX];
int result[MAX][MAX];

int main(int argc, char *argv[]){

    int size, rank;

    MPI_Init(&argc,&argv);

    MPI_Comm_size(MPI_COMM_WORLD, &size);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    memset(matrix, 0 , sizeof(matrix));
    memset(result, -1, sizeof(result));

    if(rank == 0){
        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", matrix[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    MPI_Bcast(matrix, MAX*MAX, MPI_INT, 0, MPI_COMM_WORLD);

    for(int i = 0; i<MAX; i++)
        matrix[rank][i]=rank;

    MPI_Bcast(&matrix[2], MAX, MPI_INT, 2, MPI_COMM_WORLD);

    if(rank == 1){
    printf("rank = %d\n", rank);
        for(int i = 0; i<MAX; i++)
            matrix[rank][i]=-1;

        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", matrix[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    if(rank == 2){
    printf("rank = %d\n", rank);
        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", matrix[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    MPI_Reduce(matrix, result, MAX*MAX, MPI_INT, MPI_MAX, 0, MPI_COMM_WORLD);

    if(rank == 0){
        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", result[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }


    MPI_Finalize();

    return 0;
}
```

执行上述程序，所得之结果如下：

```
$ mpirun -n 4 ./atest.out
0 0 0 0 
0 0 0 0 
0 0 0 0 
0 0 0 0 

rank = 2
0 0 0 0 
0 0 0 0 
2 2 2 2 
0 0 0 0 

rank = 1
0 0 0 0 
-1 -1 -1 -1 
2 2 2 2 
0 0 0 0 

0 0 0 0 
0 0 0 0 
2 2 2 2 
3 3 3 3 
```

需要注意的是，下面这种写法是错误的，rank所在位置的参数必须是一个确切的数值。

```
MPI_Bcast(&matrix[rank], MAX, MPI_INT, rank, MPI_COMM_WORLD);
```

所以如果你写成下面这样的程序：

```cpp
//author: baimafujinji
#include <cstdio>
#include <cstring>
#include "mpi.h"

#define MAX 4

int matrix[MAX][MAX];
int result[MAX][MAX];

int main(int argc, char *argv[]){

    int size, rank;

    MPI_Init(&argc,&argv);

    MPI_Comm_size(MPI_COMM_WORLD, &size);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    memset(matrix, 0 , sizeof(matrix));
    memset(result, -1, sizeof(result));

    if(rank == 0){
        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", matrix[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    MPI_Bcast(matrix, MAX*MAX, MPI_INT, 0, MPI_COMM_WORLD);

    for(int i = 0; i<MAX; i++)
        matrix[rank][i]=rank;

    MPI_Bcast(&matrix[rank], MAX, MPI_INT, rank, MPI_COMM_WORLD);

    if(rank == 2){

        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", matrix[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    MPI_Reduce(matrix, result, MAX*MAX, MPI_INT, MPI_MAX, 0, MPI_COMM_WORLD);

    if(rank == 0){
        for(int i = 0; i<MAX; i++){
            for (int j = 0; j < MAX; j++)
            {
                printf("%d ", result[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }


    MPI_Finalize();

    return 0;
}
```

执行上述代码之结果如下：

```
$ mpirun -n 4 ./atest.out
0 0 0 0 
0 0 0 0 
0 0 0 0 
0 0 0 0 

0 0 0 0 
0 0 0 0 
2 2 2 2 
0 0 0 0 

0 0 0 0 
1 1 1 1 
2 2 2 2 
3 3 3 3 
```

可见上代码中广播某一行的代码没有起到任何作用！这一点在实际开发中应该予以小心。

--------------------------
### 参考文献与推荐阅读材料

【1】[网址链接](http://mpitutorial.com/tutorials/mpi-scatter-gather-and-allgather/)

<span style="color:blue">**（本文完）**</span>
