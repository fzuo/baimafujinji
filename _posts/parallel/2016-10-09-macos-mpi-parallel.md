---
layout: post
title: macOS下利用MPI进行并行计算
category: 多核与并发
tags: MPI
keywords: MPI,并发编程
---


北京时间2016年6月20日下午3点，TOP500组织在法兰克福世界超算大会（ISC）上，由中国国家并行计算机工程技术研究中心研制的“神威·太湖之光”超级计算机系统登顶榜单之首，成为世界上首台运算速度超过十亿亿次的超级计算机。而此前国防科大的天河二号超级计算机保持世界超算第一的位置也已经长达六届。

超级计算机与现代科学研究与工程应用息息相关，像天气预报、新药研制、DNA分析等领域都离不开超级计算机，所以可以说超算其实无处不在。

超级计算机通常都是有众多计算节点连接而成，而每个计算节点又包括一个至多个微处理器芯片。例如，天河2号由16000个节点组成，每个节点有2颗基于Ivy Bridge-E Xeon E5 2692处理器和3个Xeon Phi,累计共有32000颗Ivy Bridge处理器和48000个Xeon Phi,总计有312万个计算核心。所以要想重复利用超算资源，还必须对程序本身进行针对性的并行优化。

但是你可以能会疑惑，如果平常无法使用到这些超级计算机，那是不是就完全不能进行并行程序开发呢？当然不是，至少现在个人电脑的处理器都是多核的，因此在你的PC上进行并行计算也是很现实的。本文就以Mac OS X系统为例来介绍MPI的配置方法，以及简单的并行计算编程。

----------------------
### MPI

MPI（Message Passing Interface）是一个跨语言的通讯协议，用于编写并行计算程序。MPI的实现版本主要有两个，即Open [MPI](https://www.open-mpi.org/)和[MPICH](http://www.mpich.org/)，正确安装和配置后，便可以在 C，C++和Fortran语言中直接调用MPI所提供的各种API。本博客之前介绍过在Windows下通过MPJ来实现MPI编程开发的基本方法（有需要的读者可以参阅本博客相关文章），下面我们就来看看在Mac OS X上如何使用MPI进行并行开发。

我们之前在介绍OpenMP（注意跟本文的OpenMPI不同）时曾经提到过，因为新版本的Xcode的编译器从GCC变成了clang，所以对非Apple的产品都不再提供默认支持了。所以我们要首先在Mac OS X上正确的安装GCC，这部分内容本博客的其他文章也已经介绍过，这里不再重复。

接下来安装OpenMPI，需要使用Homebrew（同样要确认你已经安装了它），打开Terminal，然后输入如下指令

```
brew install openmpi
```

安装过程可能需要几分钟。安装成功后，在使用mpicc编译时，需要注意，如果不做任何操作，mpicc会使用默认的gcc，也就是clang的链接来编译，使得编译出错。所以在编译之前需要设置环境变量（假设你要编写的是C++程序）：

```
export OMPI_CXX=g++-5
```

----------------------
### 编写第一个MPI程序

下面我们来写一个简单的Hello World程序，注意MPI中各个API的介绍我们这里不详细介绍，这个例子只是帮助读者熟悉MPI程序的编译和执行等步骤。首先编辑输入下面代码，并

```cpp
#include <cstdio>
#include <mpi.h>

int main (int argc, char **argv)
{
  int rank, size;

  MPI_Init (&argc, &argv);  /* starts MPI */

  MPI_Comm_rank (MPI_COMM_WORLD, &rank);    /* get current process id */
  MPI_Comm_size (MPI_COMM_WORLD, &size);    /* get number of processes */

  printf( "Hello world from process %d of %d\n", rank, size );

  MPI_Finalize();

  return 0;
}
```

然后在Terminal下输入（注意要转到你CPP源文件所在的目录下）：

```
mpic++ helloworld.cpp -o helloworld.out
mpirun -n 2 ./helloworld.out
```

第一句是编译，第二句是执行，其中 -n 2 表示使用两个核（或两个nodes）。

----------------------
### 同时使用OpenMP和MPI

我们之前已经有文章介绍过OpenMP的基本使用，下面我们想把MPI和OpenMP结合起来。来看下面这个程序，它实现的功能是即使复平面上某个矩形区域中，在某个给定的分辨率下，Mandelbrot 集合中点的数量：

```cpp
#include <cstdio>
#include <cstdlib>
#include "mpi.h"
#include "omp.h"

// return 1 if in set, 0 otherwise
int inset(double real, double img, int maxiter){
    double q = (real - 0.25)*(real - 0.25) + img*img;
    if(q*(q+(real - 0.25)) < 0.25*img*img || (real+1)*(real+1) + img*img < 1/16)
        return 1;

    double z_real = real;
    double z_img = img;

    for(int iters = 0; iters < maxiter; iters++){

        double z2_real = z_real*z_real-z_img*z_img;
        double z2_img = 2.0*z_real*z_img;
        z_real = z2_real + real;
        z_img = z2_img + img;

        if(z_real*z_real + z_img*z_img > 4.0) return 0;
    }

    return 1;
}

// main
int main(int argc, char *argv[]){
    double real_lower;
    double real_upper;
    double img_lower;
    double img_upper;
    int num;
    int maxiter;
    int num_regions = (argc-1)/6;

    //double start = omp_get_wtime();

    MPI_Init(&argc,&argv);

    for(int region=0;region<num_regions;region++){
        // scan the arguments

        sscanf(argv[region*6+1],"%lf",&real_lower);
        sscanf(argv[region*6+2],"%lf",&real_upper);
        sscanf(argv[region*6+3],"%lf",&img_lower);
        sscanf(argv[region*6+4],"%lf",&img_upper);
        sscanf(argv[region*6+5],"%i",&num);
        sscanf(argv[region*6+6],"%i",&maxiter); 

        int count=0;
        double real_step = (real_upper-real_lower)/num;
        double img_step = (img_upper-img_lower)/num;

        int size, rank;
        MPI_Comm_size(MPI_COMM_WORLD, &size);
        MPI_Comm_rank(MPI_COMM_WORLD, &rank);

        int new_num = ((real_upper-real_lower)/size) /real_step;

        if(rank != size -1){
            #pragma omp parallel
            {
                #pragma omp for schedule(dynamic) reduction(+:count) nowait
                for(int real = rank * new_num; real <= (rank+1)*new_num-1; real++){
                    for(int img=0; img<=num; img++){
                        count+=inset(real_lower + real * real_step,img_lower+img*img_step,maxiter);
                    }
                }
            }
        }
        else{
            #pragma omp parallel
            {
                #pragma omp for schedule(dynamic) reduction(+:count) nowait
                for(int real = rank * new_num; real <= num; real++){
                    for(int img=0; img<=num; img++){
                        count+=inset(real_lower+real*real_step,img_lower+img*img_step,maxiter);
                    }
                }
            }
        }

        int global_sum = 0;
        MPI_Reduce(&count, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

        if(rank == 0)
            printf("%d\n", global_sum);

    }
    MPI_Finalize();

    //double end = omp_get_wtime();

    //printf("Time = %fs\n", end-start);

    return EXIT_SUCCESS;
}
```

要编译上述程序需要在Terminal下输入：

```
mpic++ -fopenmp mandelbrot.cpp -o test_mpi
```

执行上述程序则使用：

```
mpirun -n 4 ./test_mpi -2.0 1.0 -1.0 1.0 100 10000 -1 1.0 0.0 1.0 100 10000
```

有兴趣的读者不妨尝试一下！

----------------------
### 一段轶事

2016年3三月，谷歌的AlphaGo对弈世界顶级围棋棋手韩国人李世石，引起众多关注。最终，AlphaGo以五局四胜的战绩击败李世石，一时也在网络上极大的激起了小伙伴们对人工智能的兴趣。

但是让电脑与人类比赛下棋，这也不是头一遭了，只不过通常意义下，围棋的计算量过大，如果不使用一些策略来加以优化，一般的电脑都难以胜任。相比而言，国际象棋的计算量要更小一些，所以在AlphaGo击败李世石之前，研究人员其实已经先在国际象棋上进行了尝试。只不过这段往事离现在也有些久远了。

1997年5月11日，国际象棋特级大师，世界冠军，俄罗斯人卡斯帕罗夫与IBM公司的超级电脑“深蓝”之间的终极对抗赛落下帷幕。在总共的六局比赛里，卡斯帕罗夫最终以五平一负的战绩宣告败北。

深蓝拥有有32个微处理器，根据世界超算机构的测算，深蓝的计算速度可以达到11.38×109
每秒浮点运算次数（GFLOPS），位列当年（1997年June）超算500强中的第259位。

IBM在超级计算机上丝毫没有停歇。2005年，IBM开发的蓝色基因/L（Blue Gene/L）超级计算机勇夺世界超算Top500榜首，其计算速度可达每秒钟280.6万亿次浮点运算。而笔者日常所使用的主要是第三代Blue Gene/Q超级计算机。

<span style="color:blue">**（本文完）**</span>

