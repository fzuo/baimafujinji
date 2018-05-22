---
layout: post
title: 经验分布函数
category: 数学知识
tags: Math
keywords: Math,经验分布函数
---


### 一、顺序统计量

设 $$(X_1,X_2,\cdots,X_n)$$ 是总体 $$X$$ 的一个样本。如果 $$X_i^*(i=1,2,\cdots,n)$$ 是样本 $$(X_1,X_2,\cdots,X_n)$$ 这样的函数：它总是取样本观察值 $$(x_1,x_2,\cdots,x_n)$$ 按从小到大排序后第 $$i$$ 个值为自己的观测值。那么就称 $$X_1^*,X_2^*,\cdots,X_n^*$$ 为顺序统计量。顺序统计量可以简记为

$$X_k^*=\{X_1,X_2,\cdots,X_n中第k个小的值\},\quad k=1,2,\cdots,n$$

特别地，
$$X_1^*=\min {X_1,X_2,\cdots,X_n}\\
X_n^*=\max {X_1,X_2,\cdots,X_n}$$
称$X_1^*$和$X_n^*$ 分别为样本的最小值和最大值。并称 $R=X^*_n-X^*_1$ 为样本的极差。
$$\tilde{X}=\begin{cases}
X^*_{\frac{n+1}{2}},\quad 当n为奇数时\\
\frac{1}{2}(X^*_{\frac{n}{2}} + X^*_{\frac{n}{2}+1}),\quad 当n为偶数时
\end{cases}$$
称 $\tilde{X}$ 为样本的中位数。

### 二、经验分布函数（EDF，Empirical Distribution Functions）

设 $$x_1,x_2,\cdots,x_n$$ 是总体 $$X$$ 的一组容量为 $$n$$ 的样本观测值，将它们按从小到大的顺序重新排列为 $$x^*_1,x^*_2,\cdots,x^*_n$$，对于任意实数 $$x$$，定义函数

<script type="math/tex; mode=display">F_n(x)=\begin{cases}
0,\quad x<x^*_1\\
k/n,\quad x^*_{k}\leq x<x^*_{k+1}, \quad k=1,2,\cdots,n-1\\
1,\quad x^*_n\leq x
\end{cases}</script>

则称 $F_n(x)$ 为总体 $X$ 的经验分布函数。它还可以简记为 $F_n(x)=1/n\cdot\ ^*\{x_1,x_2,\cdots,x_n\}$，其中 $^*\{x_1,x_2,\cdots,x_n\}$ 表示 $x_1,x_2,\cdots,x_n$ 中不大于 $x$ 的个数。
另外一种常见的表示形式为

<script type="math/tex; mode=display">F_n(x)=\frac{1}{n}\sum_{i=1}^nI\{x_i\leq x\}$$
其中，$I$ 是indicator function, 即
$$I\{x_i\leq x\}=\begin{cases}
1,\quad x_i\leq x\\
0,\quad otherwise
\end{cases}</script>

因此，求经验分布函数 $$F_n(x)$$ 在一点 $$x$$ 处的值，只要求出随机变量 $X$ 的 $n$ 个观测值 $x_1,x_2,\cdots,x_n$ 中小于或等于 $x$ 的个数，再除以观测次数 $$n$$ 即可。由此可见，$$F_n(x)$$ 就是在 $$n$$ 次重复独立实验中事件 $\{X\leq x\}$ 出现的频率。

经验分布函数 $$F_n(x)$$ 的图形（如下图所示）是一条呈跳跃上升的阶梯形曲线。如果样本观测值 $x_1,x_2,\cdots, x_n$ 中没有重复的数值，则每一跳跃为 $1/n$，若有重复 $$l$$ 次的值，则按 $1/n$ 的 $l$ 倍跳跃上升。图中圆滑曲线是总体 $X$ 的理论分布函数 $F(x)$ 的图形。若把经验分布函数的图形连成折线，那么它实际就是累积频率直方图的上边。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel18.png" width="360">
</p>

这和概率分布函数的性质是一致的。

### 三、格利文科定理（Glivenko Theorem）

根据大数定理可知，当试验次数增大时，事件的频率稳定于概率。那么，当试验次数增大时，表示事件 $$\{X\leq x\}$$ 出现频率的经验分布函数是否接近于表示事件 $$\{X\leq x\}$$ 出现概率的总体分布函数呢？这个问题可由格利文科定理来回答。

**格利文科定理**：设总体 $$X$$ 的分布函数 $$F(x)$$，经验分布函数 $$F_n(x)$$，则有

<script type="math/tex; mode=display"> P\{\lim_{n\rightarrow\infty}\sup_{-\infty<x<+\infty}\bracevert F_n(x)-F(x)\bracevert =0 \}=1 </script>

该定理揭示了总体 $$X$$ 的理论分布函数与经验分布函数之间的内在联系。它指出当样本容量足够大时，从样本算得的经验分布函数 $$F_n(x)$$ 与总体分布函数 $$F(x)$$ 相差的最大值也可以足够小，这就是用样本来推断总体的数学依据。



-----------------------
### 参考文献

[1] 葛余博，概率论与数理统计，清华大学出版社 <br>
[2] 李时，应用统计学，清华大学出版

<span style="color:blue">**（本文完）**</span>

<script> MathJax.Hub.Queue(["Typeset",MathJax.Hub]); </script> 
