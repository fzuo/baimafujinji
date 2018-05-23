---
layout: post
title: Skyline查询实验数据点的生成
category: 数据挖掘
tags: Skyline
keywords: Skyline,数据挖掘
---


在此前的文章里，我们介绍了Skyline查询的基本概念，并给出了一种基于NN的Skyline查询算法。Skyline查询是多维度数据库中一种非常重要的point query，它最初由 Börzsönyi 等人于2001年提出。在 Börzsönyi 等人于2001年发表的论文[1] 中，作者同时给出了三种分布类型的用于Skyline查询实验的数据集生成方法。如下图所示，这三种数据分布类型分别是独立分布数据集（下图左），正相关分布数据集（下图中）以及负相关（下图右）数据集合。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining30.png" width="580">
</p>

同时， Börzsönyi 等人亦给出了生成这些类型测试数据集的源代码，这也成为后来进行“Skyline查询”研究的人最常使用的人工合成数据生成方法（尤其是Anti-Correlated数据集，例如文献[2]、[3]、[4]）。原作者代码写于十几年前，而且是C、C++混合编程实现的，加之注释完全是用德文写的，使用起来还是有些困难。为了便于研究，笔者对其进行了微调。使其更符合C++ 11标准，如下所示：

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#include <string>
#include <math.h>

#define MAXINT 2147483647
#define sqr(a) ((a)*(a))

using namespace std;

int Statistics_Count;
double* Statistics_SumX;
double* Statistics_SumXsquared;
double* Statistics_SumProduct;


void InitStatistics(int Dimensions)
// ==============
// initialisiert Zählvariablen der Statistik
{
    Statistics_SumX = new double[Dimensions];
    Statistics_SumXsquared = new double[Dimensions];
    Statistics_SumProduct = new double[Dimensions*Dimensions];
    
    Statistics_Count = 0;
    for (int d=0; d<Dimensions; d++) {
        Statistics_SumX[d]=0.0;
        Statistics_SumXsquared[d]=0.0;
        for (int dd=0; dd<Dimensions; dd++) Statistics_SumProduct[d*Dimensions+dd] = 0.0;
    }
}


void EnterStatistics(int Dimensions,double* x)
// ===============
// registiriert den Vektor "x" für die Statistik
{
    Statistics_Count++;
    for (int d=0; d<Dimensions; d++) {
        Statistics_SumX[d] += x[d];
        Statistics_SumXsquared[d] += x[d]*x[d];
        for (int dd=0; dd<Dimensions; dd++) Statistics_SumProduct[d*Dimensions+dd] += x[d]*x[dd];
    }
}


void OutputStatistics(int Dimensions)
// ================
// gibt die Statistik aus
{
    for (int d=0; d<Dimensions; d++) {
        double E = Statistics_SumX[d] / Statistics_Count;
        double V = Statistics_SumXsquared[d]/Statistics_Count - E*E;
        double s = sqrt(V);
        printf("E[X%d]=%5.2f Var[X%d]=%5.2f s[X%d]=%5.2f\n",d+1,E,d+1,V,d+1,s);
    }
    printf("\nKorrelationsmatrix:\n");
    for (int d=0; d<Dimensions; d++) {
        for (int dd=0; dd<Dimensions; dd++) {
            double Kov = (Statistics_SumProduct[d*Dimensions+dd]/Statistics_Count) -
            (Statistics_SumX[d]/Statistics_Count) * (Statistics_SumX[dd]/Statistics_Count);
            double Cor = Kov /
            sqrt(Statistics_SumXsquared[d]/Statistics_Count - sqr(Statistics_SumX[d] / Statistics_Count)) /
            sqrt(Statistics_SumXsquared[dd]/Statistics_Count - sqr(Statistics_SumX[dd] / Statistics_Count));
            printf(" %5.2f",Cor);
        }
        printf("\n");
    }
    printf("\n");
}


double RandomEqual(double min,double max)
// ===========
// liefert eine im Intervall [min,max[ gleichverteilte Zufallszahl
{
    double x = (double)rand()/MAXINT;
    return x*(max-min)+min;
}


double RandomPeak(double min,double max,int dim)
// ==========
// liefert eine Zufallsvariable im Intervall [min,max[
// als Summe von "dim" gleichverteilten Zufallszahlen
{
    double sum = 0.0;
    for (int d=0; d<dim; d++) sum += RandomEqual(0,1);
    sum /= dim;
    return sum*(max-min)+min;
}


double RandomNormal(double med,double var)
// ============
// liefert eine normalverteilte Zufallsvariable mit Erwartungswert med
// im Intervall ]med-var,med+var[
{
    return RandomPeak(med-var,med+var,12);
}


void GenerateDataEqually(FILE* f,int Count,int Dimensions)
// ===================
// generiert in der Datei "f" "Count" gleichverteilte Datensätze
{
    InitStatistics(Dimensions);
    for (int i=0; i<Count; i++)
    {
        double x[Dimensions];
        for (int d=0; d<Dimensions; d++)
        {
            x[d] = RandomEqual(0,1);
            fprintf(f,"%8.6f ",x[d]);
        }
        EnterStatistics(Dimensions,x);
        fprintf(f,"\n");
    }
    OutputStatistics(Dimensions);
}


void GenerateDataCorrelated(FILE* f,int Count,int Dimensions)
// ======================
// generiert in der Datei "f" "Count" korrelierte Datensätze
{
    InitStatistics(Dimensions);
    double x[Dimensions];
    for (int i=0; i<Count; i++) {
    again:
        double v = RandomPeak(0,1,Dimensions);
        for (int d=0; d<Dimensions; d++) x[d] = v;
        double l = v<=0.5 ? v:1.0-v;
        for (int d=0; d<Dimensions; d++) {
            double h = RandomNormal(0,l);
            x[d] += h;
            x[(d+1)%Dimensions] -= h;
        }
        for (int d=0; d<Dimensions; d++) if (x[d]<0 || x[d]>=1) goto again;
        for (int d=0; d<Dimensions; d++) fprintf(f,"%8.6f ",x[d]);
        EnterStatistics(Dimensions,x);
        fprintf(f,"\n");
    }
    OutputStatistics(Dimensions);
}


void GenerateDataAnticorrelated(FILE* f,int Count,int Dimensions)
// ==========================
// generiert in der Datei "f" "Count" antikorrelierte Datensätze
{
    InitStatistics(Dimensions);
    double x[Dimensions];
    for (int i=0; i<Count; i++) {
    again:
        double v = RandomNormal(0.5,0.25);
        for (int d=0; d<Dimensions; d++) x[d] = v;
        double l = v<=0.5 ? v:1.0-v;
        for (int d=0; d<Dimensions; d++) {
            double h = RandomEqual(-l,l);
            x[d] += h;
            x[(d+1)%Dimensions] -= h;
        }
        for (int d=0; d<Dimensions; d++) if (x[d]<0 || x[d]>=1) goto again;
        for (int d=0; d<Dimensions; d++) fprintf(f,"%8.6f ",x[d]);
        EnterStatistics(Dimensions,x);
        fprintf(f,"\n");
    }
    OutputStatistics(Dimensions);
}


void GenerateData(int Dimensions,char Distribution,int Count, string FileName)
// ============
// generierte eine Datei mit zufälligen Daten
{
    if (Count <= 0) {
        printf("Ungültige Anzahl von Punkten.\n");
        return;
    }
    if (Dimensions < 2) {
        printf("Ungültige Anzahl von Dimensionen.\n");
        return;
    }
    switch (Distribution) {
        case 'E':
        case 'e': Distribution = 'E'; break;
        case 'C':
        case 'c': Distribution = 'C'; break;
        case 'A':
        case 'a': Distribution = 'A'; break;
        default: printf("Ungültige Verteilung.\n"); return;
    }
    
    FILE* f = fopen(FileName.data(),"wt");
    if (f == NULL) {
        printf("Kann Datei \"%s\" nicht anlegen.\n",FileName.data());
        return;
    }
    fprintf(f,"%d %d\n",Count,Dimensions);
    switch (Distribution) {
        case 'E': GenerateDataEqually(f,Count,Dimensions); break;
        case 'C': GenerateDataCorrelated(f,Count,Dimensions); break;
        case 'A': GenerateDataAnticorrelated(f,Count,Dimensions); break;
    }
    fclose(f);
    printf("%d Punkte generiert, Datei \"%s\".\n",Count,FileName.data());
}
```
有了上面这些函数，那么你在其他函数中做简单调用就能生成想要的测试数据集了。下面我们给出一个简单的例子，分别生成两个2D的含有500个点的数据集，一个是自相关的，一个是负相关的。代码如下：

```cpp
int main(int argc, char** argv)
{
    //Verteilung = E(qually) | C(orrelated) | A(nti-correlated);
    
    GenerateData(2,'A',500, "data_anti_corr");
    GenerateData(2,'C',500, "data_correlate");

    return 0;
}
```

上述代码会生成两个名为“data_anti_corr”和“data_correlate”的数据文件，为了更形象地展示所生成的数据集，我们用软件绘制了它们的散点图，如下所示，其中左图是自相关数据集，右图是负相关数据集。

<p align="center">
<img src="https://fzuo.github.io/assets/img/datamining/datamining31.png" width="520">
</p>

----------------------
### 参考文献

[1] Stephan Börzsönyi , Donald Kossmann , Konrad Stocker, The Skyline Operator, Proceedings 17th International Conference on Data Engineering: 421–430. , Heidelberg, Germany, 2001.

[2] Y. Tao, L. Ding, X. Lin, and J. Pei. Distance-based representative skyline. ICDE, 2009.

[3] D. Nanongkai, A. D. Sarma, A. Lall, R. J. Lipton, and J. Xu. Regret-Minimizing Representative Databases. The 36th International Conference on VLDB, September 13-17, 2010, Singapore.

[4] T. K. Faulkner, W. Brackenbury, A. Lall. k-Regret Queries with Nonlinear Utilities. The 42nd International Conference on VLDB, September 5-9, 2016, New Delhi, India.


<span style="color:blue">**（本文完）**</span>