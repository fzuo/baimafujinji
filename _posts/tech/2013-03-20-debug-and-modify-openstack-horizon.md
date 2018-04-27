---
layout: post
title: 调试和修改OpenStack中的Horizon部分
category: 技术
tags: OpenStack
description: 在OpenStack二次开发中，Horizon肯定是常常要修改的部分，简要说一下我自己调试和修改的方式
---

# Hessian矩阵与多元函数极值

海塞矩阵（Hessian Matrix），又译作海森矩阵，是一个多元函数的二阶偏导数构成的方阵。尽管它是一个具有悠久历史的数学成果，但是在机器学习和图像处理（例如SIFT和SURF特征检测）中，我们也常常遇到它。所以本文就来向读者道一道Hessian Matrix的来龙去脉。本文的主要内容包括：

- **多元函数极值问题**
- **泰勒展开式与Hessian矩阵**

-------------------

## 多元函数极值问题

回想一下我们是如何处理一元函数求极值问题的。例如，$f(x)=x^2$，我们会先求一阶导数，即$f'(x)=2x$，根据费马定理极值点处的一阶导数一定等于 $0$。但这仅是一个必要条件，而非充分条件。对于$f(x)=x^2$来说，函数的确在一阶导数为零的点取得了极值，但是对于$f(x)=x^3$来说，显然只检查一阶导数是不足以下定论的。

这时我们需要再求一次导，如果二阶导数 $f''<0$，那么说明函数在该点取得局部极大值；如果二阶导数 $f''>0$，则说明函数在该点取得局部极小值；如果 $f''=0$，则结果仍然是不确定的，我们就不得不再通过其他方式来确定函数的极值性。

如果要在多元函数中求极值点，方法与此类似。作为一个示例，不妨用一个三元函数 $f=f(x,y,z)$ 来作为示例。首先要对函数中的每个变量分别求偏导数，这会告诉我们该函数的极值点可能出现在哪里。即
\[
\ \frac{\partial f}{\partial x}=0\\
\ \frac{\partial f}{\partial y}=0\\
\ \frac{\partial f}{\partial x}=0
\]
接下来，要继续求二阶导数，此时包含混合偏导数的情况一共有 $9$ 个，如果用矩阵形式来表示的话就得到
$$\mathbf{H}=\begin{bmatrix} \frac{\partial^2f}{\partial x\partial x} & \frac{\partial^2f}{\partial x\partial y} & \frac{\partial^2f}{\partial x\partial z} \\ \frac{\partial^2f}{\partial y\partial x} & \frac{\partial^2f}{\partial y\partial y} & \frac{\partial^2f}{\partial y\partial z} \\\frac{\partial^2f}{\partial z\partial x}&\frac{\partial^2f}{\partial z\partial y}&\frac{\partial^2f}{\partial z\partial z} \end{bmatrix}$$

这个矩阵就称为Hessian矩阵。当然上面所给出的仅仅是一个三阶的Hessian矩阵。稍作扩展，我们可以对一个在定义域内二阶连续可导的实值多元函数 $f(x_1, x_2, \cdots, x_n)$ 定义其Hessian矩阵$\mathbf{H}$如下
$$\mathbf{H}=\begin{bmatrix} 
\frac{\partial^2f}{\partial x_1^2} & \frac{\partial^2f}{\partial x_1\partial x_2} & \cdots &\frac{\partial^2f}{\partial x_1\partial x_n} \\
 \frac{\partial^2f}{\partial x_2\partial x_1} & \frac{\partial^2f}{\partial x_2^2} &  \cdots &\frac{\partial^2f}{\partial x_2\partial x_n} \\ \vdots & \vdots & \ddots & \vdots \\
\frac{\partial^2f}{\partial x_n\partial x_1}&\frac{\partial^2f}{\partial x_n\partial x_2}&\cdots &\frac{\partial^2f}{\partial x_n^2} \end{bmatrix}$$

当一元函数的二阶导数等于 $0$ 时，我们并不能确定函数在该点的极值性。类似地，面对Hessian矩阵，仍然存在无法断定多元函数极值性的的情况，即当Hessian矩阵的行列式为 $0$ 时，我们无法确定函数是否能取得极值。甚至我们可能会得到一个鞍点，也就是一个既非极大值也非极小值的的点。
<center>
<img src="http://img.blog.csdn.net/20160416210534270" width=350>
</center>
基于Hessian矩阵，就可以判断多元函数的极值情况了，结论如下

 - 如果是正定矩阵，则临界点处是一个局部极小值 
 - 如果是负定矩阵，则临界点处是一个局部极大值
 - 如果是不定矩阵，则临界点处不是极值

<br>如何判断一个矩阵是否是正定的，负定的，还是不定的呢？一个最常用的方法就是顺序主子式。实对称矩阵为正定矩阵的充要条件是的各顺序主子式都大于零。当然这个判定方法的计算量比较大。对于实二次型矩阵还有一个判定方法：实二次型矩阵为正定二次型的充要条件是的矩阵的特征值全大于零。为负定二次型的充要条件是的矩阵的特征值全小于零，否则是不定的。

如果你对二次型的概念仍然不很熟悉，这里也稍作补充。定义含有 $n$
个变量 $x_1,x_2,\cdots,x_n$ 的二次齐次函数
$$f(x_1,x_2,\cdots,x_n)=a_{11}x_1^2+a_{22}x_2^2+\cdots+a_{nn}x_n^2+2a_{12}x_1x_2+2a_{13}x_1x_3+\cdots+2a_{n-1,n}x_{n-1}x_n$$
为二次型。取 $a_{ij}=a_{ji}$，则 $2a_{ij}x_ix_j+a_{ji}x_jx_i$，于是上式可以写成
$$\begin{align}f=&a_{11}x_1^2+a_{12}x_1x_2+\cdots+a_{1n}x_1x_n\\
&+a_{21}x_2x_1+a_{22}x_2^2+\cdots+a_{2n}x_2x_n+\cdots\\
&+a_{n1}x_nx_1+a_{n2}x_nx_2+\cdots+a_{nn}x_n^2\\
=&\sum_{i,j=1}^na_{ij}x_ix_j\end{align}$$
更进一步，如果用矩阵对上式进行改写，则有
$$\begin{align}f=&x_1(a_{11}x_1+a_{12}x_2+\cdots+a_{1n}x_n)+\\
&x_2(a_{21}x_1+a_{22}x_2+\cdots+a_{2n}x_n)+\cdots+\\
&x_n(a_{n1}x_1+a_{n2}x_2+\cdots+a_{nn}x_n)\\=&(x_1,x_2,\cdots,x_n)
\begin{bmatrix}a_{11}x_1+a_{12}x_2+\cdots+a_{1n}x_n\\
a_{21}x_1+a_{22}x_2+\cdots+a_{2n}x_n\\\vdots\\
a_{n1}x_1+a_{n2}x_2+\cdots+a_{nn}x_n
\end{bmatrix}\\
=&(x_1,x_2,\cdots,x_n)
\begin{bmatrix}
a_{11}&a_{12}&\cdots&a_{1n}\\
a_{21}&a_{22}&\cdots&a_{2n}\\
\vdots&\vdots&\ddots&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}
\end{bmatrix}\begin{bmatrix}x_1\\x_2\\\vdots\\x_n\end{bmatrix}
\end{align}$$
记
$$\mathbf{A}=\begin{bmatrix}a_{11}&a_{12}&\cdots&a_{1n}\\
a_{21}&a_{22}&\cdots&a_{2n}\\
\vdots&\vdots&\ddots&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}
\end{bmatrix},\mathbf{x}=
\begin{bmatrix}x_1\\x_2\\\vdots\\x_n\end{bmatrix}$$
则二次型可记作 $f=\mathbf{x}^T\mathbf{A}\mathbf{x}$，其中 $\mathbf{A}$为对称阵。
设有二次型 $f=\mathbf{x}^T\mathbf{A}\mathbf{x}$，如果对任何 $x\not=0$，都有 $f>0$，则称 $f$ 为正定二次型，并称对称矩阵 $\mathbf{A}$ 是正定的；如果对任何 $x\not=0$，都有 $f<0$，则称 $f$ 为负定二次型，并称对称矩阵 $\mathbf{A}$ 是负定的。
正定矩阵一定是非奇异的。对阵矩阵 $\mathbf{A}$ 为正定的充分必要条件是： $\mathbf{A}$ 的特征值全为正。由此还可得到下面这个推论：对阵矩阵 $\mathbf{A}$ 为正定的充分必要条件是 $\mathbf{A}$ 的各阶主子式都为正。如果将正定矩阵的条件由 $\mathbf{x}^T\mathbf{A}\mathbf{x}>0$ 弱化为 $\mathbf{x}^T\mathbf{A}\mathbf{x}\geq0$，则称对称矩阵 $\mathbf{A}$ 是半正定的。

---------
## 泰勒展开式与Hessian矩阵

主页君已经在之前的《图像处理中的数学原理详解》系列文章中介绍过泰勒展开式了。但那个时候我们给出的是一元函数的泰勒公式，不妨先来复习一下。
设一元函数 $f(x)$ 在包含点$x_0$的开区间 $(a,b)$ 内具有 $n+1$ 阶导数，则当 $x\in (a,b)$ 时，有
$$f(x)=f(x_0)+f'(x_0)(x-x_0)+\frac{f''(x_0)}{2!}(x-x_0)^2+\cdots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n+R_n(x)$$
其中
$$R_n(x)=\frac{f^{(n+1)}(\xi)}{(n+1)!}(x-x_0)^{n+1}$$
并且，$\xi$ 在 $x$ 和 $x_0$之间，这被称作是拉格朗日余项。上式被称为 $f(x)$ 的 $n$ 阶泰勒公式。在不需要余项的精确表达式时，$R_n(x)$ 可以记作 $o[(x-x_0)^n]$，这被称为是皮亚诺余项。

现在我们把上面这个结论稍微做一下推广，从而给出二元函数的泰勒公式。设二元函数 $z=f(x,y)$ 在点 $(x_0,y_0)$ 的某一邻域内连续且有直到 $n+1$ 阶的连续偏导数，则有
$$\begin{align}f(x,y)&=f(x_0,y_0)+[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]f(x_0,y_0)\\
&+\frac{1}{2!}[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^2f(x_0,y_0)+\cdots +\\
&+\frac{1}{n!}[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^nf(x_0,y_0)\\
&+\frac{1}{(n+1)!}[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^{(n+1)}f[x_0+\theta(x-x_0),y_0+\theta(y-y_0)]\end{align}$$
其中，$0<\theta<1$，记号
$$[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]f(x_0,y_0)$$
表示
$$(x-x_0)f_x(x_0,y_0)+(y-y_0)f_y(x_0,y_0)$$
记号
$$[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^2f(x_0,y_0)$$
表示
$$(x-x_0)^2f_{xx}(x_0,y_0)+2(x-x_0)(y-y_0)f_{xy}(x_0,y_0)+(y-y_0)^2f_{yy}(x_0,y_0)$$
一般地，记号
$$[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^mf(x_0,y_0)$$
表示
$$\sum_{p=0}^m{C_m^p (x-x_0)^p (y-y_0)^{(m-p)}\frac{\partial^mf}{\partial x^p\partial y^{(m-p)}}}\bigg|_{(x_0,y_0)}$$
当然，我们可以用一种更加简洁的形式来重写上面的和式，则有
$$\begin{align}f(x,y)&=\sum_{k=0}^n\frac{1}{k!}[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^kf(x_0,y_0)\\
&+\frac{1}{(n+1)!}[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^{(n+1)}f[x_0+\theta(x-x_0),y_0+\theta(y-y_0)], (0<\theta<1)
\end{align}$$
当余项$R_n(x,y)$采用上面这种形式时称为拉格朗日余项，如果采用皮亚诺余项，则二元函数的泰勒公式可以写成
$$f(x,y)=\sum_{k=0}^n\frac{1}{k!}[(x-x_0)\frac{\partial}{\partial x}+(y-y_0)\frac{\partial}{\partial y}]^kf(x_0,y_0)+o(\rho ^n)$$
特别低，对于一个多维向量 $\mathbf{X}$, 以及在点 $\mathbf{X}_0$ 的邻域内有连续二阶偏导数的多元函数 $f(\mathbf{X})$ ，可以写出该函数在点 $\mathbf{X}_0$ 处的（二阶）泰勒展开式
$$f(\mathbf{X})=f(\mathbf{X}_0)+(\mathbf{X}-\mathbf{X}_0)^T\nabla f(\mathbf{X}_0)+\frac{1}{2!}(\mathbf{X}-\mathbf{X}_0)^T\nabla^2 f(\mathbf{X}_0)(\mathbf{X}-\mathbf{X}_0)+o(\|\mathbf{X}-\mathbf{X}_0\|^2)$$
其中，$o(\|\mathbf{X}-\mathbf{X}_0\|^2)$ 是高阶无穷小表示的皮亚诺余项。而 $\nabla^2 f(\mathbf{X}_0)$ 显然就是一个Hessian矩阵。所以上述式子也可以写成
$$f(\mathbf{X})=f(\mathbf{X}_0)+(\mathbf{X}-\mathbf{X}_0)^T\nabla f(\mathbf{X}_0)+\frac{1}{2}(\mathbf{X}-\mathbf{X}_0)^T\mathbf{H}(\mathbf{X}_0)(\mathbf{X}-\mathbf{X}_0)+o(\|\mathbf{X}-\mathbf{X}_0\|^2)$$

我们已经知道对于 $n$ 元函数 $u=f(x_1,x_2,\cdots ,x_n)$在点 $M$ 处有极值，则有
$$\nabla f(M)=\left \{\frac{\partial f}{\partial x_1},\frac{\partial f}{\partial x_2},\cdots, \frac{\partial f}{\partial x_n}\right\}_M=0$$
也就是说这是一个必要条件，而充分条件则由上一节中之结论给出 。

