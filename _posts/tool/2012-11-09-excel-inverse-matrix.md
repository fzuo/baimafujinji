---
layout: post
title: 用Execel求解一个矩阵的逆矩阵
category: 技巧与工具
tags: Excel
keywords: Excel,矩阵求逆
---

Excel中有很多执行线性代数运算的函数，只要轻点鼠标，各种计算都手到擒来。这一部分，我们将介绍矩阵求逆的方法。

对于n阶方阵A，如果另外有一个n阶方矩阵B，使得A×B=E，E为单位矩阵，则A和B互为逆矩阵。比如下面的例子，读者可以采取[《用Execel计算两个矩阵的乘法》](https://fzuo.github.io/2012/11/08/excel-matrix-mul.html)中提供的方法来验证它们相乘的结果是一个单位矩阵。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel06.png" width="200">
</p>

注意根据线性代数的知识：

1. 只有方阵才有逆矩阵；

2. A是可逆矩阵的充分必要条件是矩阵的行列式∣A∣≠0，即可逆矩阵就是非奇异矩阵。

首先，我们在EXCEL中输入一个待求逆的矩阵，如下图所示，其中蓝色部分表示A矩阵。然后再选定一个与A同样大小的区域作为显示计算结果的部分，如图中黄色部分所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel07.png" width="480">
</p>

EXCEL中提供的矩阵求逆函数是MINVERSE，所以在选定显示计算结果的区域之后，我们就可以在公式栏中编辑公式，如果你看不到公示栏，那么先输入等号“=”，EXCEL就会知道你将要输入的是一个公式。然后在函数MINVERSE的参数部分，选定蓝色的区域，如下所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel08.png" width="300">
</p>

主要要把括号输入完整。然后，同时按下【Ctrl + Shift + Enter】， 就大功告成啦！

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel09.png" width="350">
</p>
