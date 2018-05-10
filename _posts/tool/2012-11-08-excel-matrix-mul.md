---
layout: post
title: 用Execel计算两个矩阵的乘法
category: 技巧与工具
tags: Excel
keywords: Excel,矩阵乘法
---


Excel中有很多执行线性代数运算的函数，只要轻点鼠标，各种计算都手到擒来。但是使用这些函数在操作上其实还是有点trick的，今天我们就来演示一下如何在Excel中执行两个矩阵的乘法。
假设我们现在有这样两个矩阵

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel01.png" width="200">
</p>

注意根据线性代数的知识：

1. 矩阵A×B 和B×A是不同的；

2. 其次，如果矩阵A的大小是 m×n, 矩阵B的大小必须是 n×k，那么A×B 的计算才能执行，而且结果矩阵的大小应该是m×k，其中m和k的值可以相等，也可以不相等。

所以下面我们首先在EXCEL中将这两个矩阵输入，如下图所示，其中蓝色是矩阵A，黄色是矩阵B，然后我们选定一个2×2的区域，作为用来显示结果的区域。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel02.png" width="350">
</p>

然后，要注意，这时我们需要按下 F2 键 以启动公式编辑，这样做的目的是为了让这个2×2的区域都可以显示出最终的计算矩阵的元素。而我们这时需要用来进行矩阵计算的公式是MMULT。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel03.png" width="420">
</p>

然后，可以用数遍，选定矩阵A所在的区域（即图中的蓝色部分），再选定矩阵B所在的区域（即图中的黄色部分），如下所示

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel04.png" width="420">
</p>

最后同时按下 【Ctrl + Shift + Enter】， 就大功告成啦！

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel05.png" width="360">
</p>
