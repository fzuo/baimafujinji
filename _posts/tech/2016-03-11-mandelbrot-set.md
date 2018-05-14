---
layout: post
title: 曼德勃罗集合与其编程实现
category: 编程与开发
tags: 分形
keywords: 曼德勃罗集合
---


## 从科赫雪花谈起

设想一个边长为1的等边三角形（如下图所示），取每边中间的三分之一，接上去一个形状完全相似的但边长为其三分之一的三角形，结果是一个六角形。现在取六角形的每个边做同样的变换，即在中间三分之一接上更小的三角形，以此重复，直至无穷。外界的变得原来越细微曲折，形状接近理想化的雪花。这个图形的名字叫柯赫雪花。1904年，瑞典数学家柯赫首次描述了一种后来以其名字命名的曲线——柯赫曲线，而柯赫雪花就是由柯赫曲线所围成的一个封闭的图形。柯赫雪花有很多奇妙的性质。例如，柯赫雪花的周长趋于无穷，而柯赫雪花的面积却是有限的！此外，柯赫雪花也体现了一种（递归的）自相似性，即当把图形的局部放大，它会呈现出一种与整体（或整体的部分）之间的惊人形似性。

<p align="center">
<img src="https://fzuo.github.io/assets/img/vs_msb802/mand01.png" width="560">
</p>

## 分形

尽管类似于柯赫雪花这样的自相似图形早已经出现。但是真正把这些问题系统地发展成一门学问，则是在二十世纪下半页。数学家本华·曼德勃罗（Benoit B. Mandelbrot）把这些部分与整体以某种方式相似的形体称为分形（fractal）。1975年，他创立了分形几何学（Fractal Geometry）。在此基础上，形成了研究分形性质及其应用的科学，称为分形理论（Fractal Theory）。因此，曼德勃罗也被称为“分形学之父”。现在，分形理论已经发展成为一门十分风靡和活跃的新理论、新学科，特别是把分形理论和混沌理论结合之后，更是衍生出一大片广阔的研究天地。分形的世界与我们平常所研究的几何学中很多直观的常识之间具有巨大的冲突。例如，平常我们说一个几何图形的维度，那么这个维度一般都应该是整数。例如平面上一个矩形就是二维的，空间中一个球体就是三维的。但是在分析几何中，几何图形的维度都是不是整数而是分数。这个维度又称为豪斯多夫维或豪斯多夫-贝塞科维奇维（Hausdorff-Becikovich Dimesion），它是由数学家豪斯多夫于1918年引入的。通过豪斯多夫维可以给一个任意复杂的点集合比如分形赋予一个维度。

## 曼德勃罗集合（Mandelbrot Set）

曼德勃罗集合（Mandelbrot Set）或曼德勃罗复数集合，是一种在复平面上组成分形的点的集合，因由曼德勃罗提出而得名。曼德博集合可以使复二次多项式 进行迭代来获得。其中，c是一个复参数。对于每一个c，从 z = 0 开始对fc(z)进行迭代。序列 的值或者延伸到无限大，或者只停留在有限半径的圆盘内（这与不同的参数c有关）。曼德布洛特集合就是使以上序列不延伸至无限大的所有c点的集合。

最后，我们给出一个利用C语言生成Mandelbrot集合并绘制图形的程序（该程序来自文献【1】）：

```c
#include <stdio.h>
#include <stdlib.h>
#include <complex.h>

#define width_size      800
#define height_size     600
#define Maxval          255

static const float orig_x = width_size * 2/3;
static const float orig_y = height_size * 1/2;

typedef struct _pixel {
    unsigned char r;
    unsigned char g;
    unsigned char b;
} pixel;

static const pixel dim_gray = { 105, 105, 105 };

static unsigned char iteration(int x, int y)
{
    const int limit = Maxval + 1;
    int i;
    complex c = ((x - orig_x) / (width_size / 3)) +
    ((orig_y - y) / (height_size / 2)) * I;
    complex z = 0;
    
    for (i = 0; i < limit; i++) {
        /* basic formula */
        z = z * z + c;
        if (creal(z) > 2 || cimag(z) > 2)
            break;
    }
    return (unsigned char) (i == limit ? 0 : i);
}


int main()
{
    FILE *f = fopen("mandelbrot.ppm", "w+");
    
    /* PPM header */
    fprintf(f,
            "P6\n"      /* PPM magic number */
            "#Mandelbrot Set\n"
            "%d "       /* width, in ASCII decimal */
            "%d\n"      /* height, in ASCII decimal */
            "%d\n",     /* maximum color value, in ASCII decimal */
            width_size, height_size, Maxval);
    
    /* Write every pixel generated by Mandelbrot Set */
    for (int i = 0; i < height_size; i++) {
        for (int j = 0; j < width_size; j++) {
            unsigned char iter = iteration(j, i);
            if (iter) {
                pixel p = {
                    .r = iter,
                    .g = (float) abs(j - orig_x) / width_size * Maxval,
                    .b = (float) abs(i - orig_y) / height_size * Maxval };
                fwrite(&p, sizeof(pixel), 1, f);
            } else {
                fwrite(&dim_gray, sizeof(pixel), 1, f);
            }
        }
    }
    
    fclose(f);
    return 0;
}
```

上述程序所生成的图像结果如下图所示，需要补充说明的是：该图像文件格式为ppm，在Windows下你可以使用Photoshop来查看这种类型的图像文件，在OS X系统下你可以使用免费的GIMP软件来查看它。

<p align="center">
<img src="https://fzuo.github.io/assets/img/vs_msb802/mand02.png" width="450">
</p>

-----------------------------
### 参考文献：

【1】[网页链接](http://blog.linux.org.tw/~jserv/archives/2011/09/_mandelbrot_set.html)