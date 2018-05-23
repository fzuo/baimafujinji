---
layout: post
title: 空间数据挖掘中的Skyline查询
category: 数据挖掘
tags: 数据挖掘
keywords: Skyline,数据挖掘
---


### 〇、引子——空间数据库


A **spatial database** is a database that is optimized to store and query data that represents objects defined in a geometric space. Most spatial databases allow representing simple geometric objects such as points, lines and polygons.

Spatial data management refers to applications that rely mostly on two-dimensional or layered two-dimensional data. There are two main types of query for spatial database:

- Point query : find all objects that contain a given search point;
- Region query : find all objects that overlap a given search region.

PAMs (point access methods), have primarily been designed to perform spatial searches on point databases;

SAMs (spatial access methods), can manage extended objects, e.g. lines, polygons.

从定义可见空间数据库中存储的是几何空间中的对象，如果空间数据库中存储的对象只考虑点的话，那么这样的空间数据库又是一个多维数据库，因为空间中的一个点就是一个多维度的数据（Multidimensional Data）。所以这时我们主要关注的query类型就是point query。

### 一、Skyline查询

Skyline query是多维度数据库中一种非常重要的point query，它最初由 Börzsönyi 等人于2001年提出。显然一个数据库中的数据对象（也就是空间中的点）可能有成千上万个，但是我们往往对其中一些更感兴趣。Skyline就是定义这里的“更感兴趣”的一种方式。如下图所示是美国纽约曼哈顿区的天际线（Skyline)。显然这里的建筑有很多很多，但放眼望去我们所能看见的（也就是构成天际线的）建筑其实是相当有限的。这些建筑要么是离我们特别近，要么是特别高大。换言之，也只有那些无法被完全遮挡的建筑才能被看到。Skyline query正是从这个想法出发提出的一种point query。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining25.jpg" width="480">
</p>

对应于前面讨论的“被遮挡”这个概念，我们首先定义domination(或point domination)这个概念：

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining26.png" width="500">
</p>

例如在下图中，点a dominates b 和 e；点 k dominates e and l；点 i dominates 除了 a 和 k 的其余所有点。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining27.png" width="320">
</p>

基于domination的概念，我们可以来定义skyline points：

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining28.png" width="480">
</p>

例如，在上图中，数据集中的其它所有点都被点集\{a, k, i\} 所支配（dominated）。换言之，在子集\{a, k, i\}中的任何点都没有被全集中的其它点所支配。因此{a, k, i}就是这个数据集的 skyline points。


Skyline一个经典的例子是假设去Nassau海滩旅游，想找一个既便宜又靠近海滩的旅馆，一般情况下越靠近海滩的旅馆价格越高，所以不能返回一个最好的结果，只能返回一些用户可能感兴趣的旅馆，这些旅馆在价格和距离两个方面都不比其它旅馆差，这些不被支配的旅馆就是Skyline。The preference function in our example is "minimum price and minimum distance". The dataset may not have one single data point that satisfies both these desirable properties. The user is presented with a set of interesting points that partly satisfy the imposed constraints. 而对应上图中的例子，点a具有离海滩最近的距离，点k具有最低的价格，尽管点i既没有离海滩最近的距离也没有最低的价格，但是相对于点a而言，它的价格更低，而相对于点k而言，它距离海滩更近。相比之下，其它点则没有什么特别的亮点。


### 二、一个经典的Skyline查询算法


可以进行Skyline查询的算法有很多。作为一个示例，我们来介绍Kossmann等人在2002年提出的基于最近邻搜索的Skyline查询算法。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining29.png" width="580">
</p>

如上图所示，从左到右：

1. 首先，我们搜索原点的NN点，得到i，那么左图中的区域2和4就没有必要搜索了。
2. 继续在区域1中搜索原点的NN点，得到a，那么中图中的区域1和4就没有必要搜索了。
3. 在右图中示出的未被搜索区域中继续重复执行上面的步骤直到没有新的NN点为止。

对于这个算法的更多细节，包括推广到更高维度时的情况，可以参阅文献[2]以了解更多。


### 三、一个简单的Skyline查询实现


下面我们在C++中编程实现一个简单的Skyline查询程序。程序的主函数首先读入一个名为data_file.dat的文件（存储了多维数据集），文件的示例内容如下，第一行表示文件中共有10条记录，每条记录共有3个维度，此后每行就表示具体的一条记录。


```
10 3
10 10 10
100 10 40
10 100 15
30 90 80
50 50 50
90 20 30
30 30 10
25 15 15
35 20 30
18 25 35
```

示例代码如下（注意上述代码中省略了部分内存释放的代码，实际应用时应注意补全这部分代码）：

```cpp
#include <iostream>

using namespace std;

#define MAX_N 10000000

struct point
{
    int d;
    double* a;
};

// checks if p1 dominates p2
// assumes p1.d == p2.d
// points dominate themselves
int dominates(struct point p1, struct point p2)
{
    int i;
    
    for(i = 0; i < p1.d; ++i)
        if (p1.a[i] < p2.a[i])
            return 0;
    
    return 1;
}

bool readpoints(const char* filename, struct point *p, int *EN, int* DEE)
{
    int i, j, D;
    int N;
    double x;
    FILE* fp;

    // Read in all the points
    fp = fopen(filename, "r");
    
    if (!fp) {
        fprintf(stderr, "failed to open data file for reading\n");
        return 1;
    }
    
    fscanf(fp, "%d %d\n", &N, &D);
    *EN = N; *DEE = D;
    for(i = 0; i < N; ++i)
    {
        p[i].a = (double*)malloc(sizeof(double) * D);
        //printf("%f\n", *p[i].a);
        for(j = 0; j < D; ++j)
        {
            fscanf(fp, "%lf ", &x);
            p[i].a[j] = x;
        }
        p[i].d = D;
    }
    
    fclose(fp);
    return 0;
}

int skyline(int N, struct point *p, struct point *skyline)
{
    int i, j, dominated, index = 0, m;
    struct point pt;
    
    for(i = 0; i < N; ++i)
    {
        dominated = 0;
        pt = p[i];
        
        // check if pt is dominated by the skyline so far
        for(j = 0; j < index && !dominated; ++j)
            if (dominates(skyline[j], pt))
                dominated = 1;
        
        if (!dominated)
        {
            // eliminate any points in current skyline that it dominates
            m = index;
            index = 0;
            for(j = 0; j < m; ++j)
                if (!dominates(pt, skyline[j]))
                    skyline[index++] = skyline[j];
            
            // add this point as well
            skyline[index++] = pt; 
        }
    }  
    
    return index;
}

bool findSkyline(const char* filename, struct point *p, int *S, int* DEE)
{
    int i, j, N, D;
    double x;
    FILE* fp;
    struct point* hull;
    
    // create skyline filename
    const char* skyline_file = "skyline_file.data";
    

    fp = fopen(filename, "r");
    if (fp == NULL){
        printf("File %s not found.\n", filename);
        return 1;
    }
        
    //Read in the original data file (entire data set)
    fscanf(fp, "%d %d\n", &N, &D);
    for(i = 0; i < N; ++i)
    {
        p[i].a = (double*)malloc(sizeof(double) * D);
        for(j = 0; j < D; ++j)
        {
            fscanf(fp, "%lf ", &x);
            p[i].a[j] = x;
        }
        p[i].d = D;
    }
    fclose(fp);
        
    // construct skyline
    hull = (struct point*)malloc(sizeof(struct point) * N);
    N = skyline(N, p, hull);
        
    // write skyline
    fp = fopen(skyline_file, "w");
    if (!fp) {
        fprintf(stderr, "failed to open data file for reading\n");
        return 1;
    }
    
    fprintf(fp, "%d %d\n", N, D);
    *S = N; *DEE = D;
    
    for(i = 0; i < N; ++i)
    {
        p[i].a = (double*)malloc(sizeof(double) * D);
        for(j = 0; j < D; ++j)
        {
            fprintf(fp, "%lf ", hull[i].a[j]);
            p[i].a[j] = hull[i].a[j];
        }
        
        fprintf(fp, "\n");
        p[i].d = D;
    }
    
    fclose(fp);
    free(hull);
    return 0;
}


int main(int argc, const char * argv[]) {

    int S, N, D;
    struct point *pAll;
    struct point *skyline;
    
    const char* filename = "data_file.dat";
    
    pAll = (struct point*)malloc(sizeof(struct point) * MAX_N);
    if(readpoints(filename, pAll, &N, &D)==1)
        return 0;
    
    //Output all points in data set
    cout << "All points in data set:\n";
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < D; j++) {
            cout<<pAll[i].a[j]<<" ";
        }
        cout << "\n";
    }
    
    skyline = (struct point*)malloc(sizeof(struct point) * MAX_N);
    if(findSkyline(filename, skyline, &S, &D)==1)
        return 0;
    
    //Output skyline points in data set
    cout << "Skyline points in data set:\n";
    for (int i = 0; i < S; i++) {
        for (int j = 0; j < D; j++) {
            cout<<skyline[i].a[j]<<" ";
        }
        cout << "\n";
    }
    
    return 0;
}
```

执行上述代码，所得之结果如下：

```
All points in data set:
10 10 10 
100 10 40 
10 100 15 
30 90 80 
50 50 50 
90 20 30 
30 30 10 
25 15 15 
35 20 30 
18 25 35 
Skyline points in data set:
100 10 40 
10 100 15 
30 90 80 
50 50 50 
90 20 30 
Program ended with exit code: 0
```

可见程序成功地筛选出了其中的Skyline点。

-----------------------
### 参考文献与推荐阅读


[1] Volker Gaede, Oliver Günther, Multidimensional Access Methods, ACM Computing Surveys, Vol. 30, No. 2, June 1998

[2] Donald Kossmann, Frank Ramsak, Steffen Rost, Shooting Stars in the Sky: An Online Algorithm for Skyline Queries, Proceedings of the 28th VLDB Conference, Hong Kong, China, 2002

[3] Stephan Börzsönyi , Donald Kossmann , Konrad Stocker, The Skyline Operator, Proceedings 17th International Conference on Data Engineering: 421–430. , Heidelberg, Germany, 2001.

[4] 魏小娟, 杨婧, 李翠平, 陈红, Skyline 查询处理, 软件学报, Vol.19, No.6, June 2008, pp.1386−1400

[5] [网址链接](https://blog.acolyer.org/2016/07/18/progressive-skyline-computation-in-database-systems/)

<span style="color:blue">**（本文完）**</span>
