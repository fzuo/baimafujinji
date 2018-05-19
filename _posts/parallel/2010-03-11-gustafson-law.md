---
layout: post
title: Gustafson定律
category: 多核与并发
tags: Gustafson定律
keywords: Gustafson定律
---

Gustafson定律由美国计算机科学家John Gustafson及其合作者于1988年提出。Amdahl定律的隐含前提是串行部分所占比例是固定的，而Gustafson有以下两点假设：

- 问题规模增加后，不能并行执行部分的规模是固定的，换句话说在多处理器上串行执行的时间是固定的。
- 程序在多处理器上执行的总时间是常数。

1988年，Sandia实验室在具有 1024个处理器的超立方体结构上观察到了3个实际应用程序发生了超线性加速现象，3个应用实际可以达到的加速比分别为1021、1020、1016。这个结果可以说是非常振奋人心的，它表明了在实际情况中是有可能得到线性加速比的，这和Amdahl定律预测并不符合。

仍然假设串行部分占总单处理器上的顺序执行时间的比例为 f，在单处理器上顺序执行时间为$$t_s$$ 。这样在多处理器上执行时，将执行时间分为两部分：顺序部分执行时间为 $$s = ft_s$$，可并行化部分执行时间为 $$r = (1-f)t_s/p$$ ，在多处理器上并行执行时总的时间为 $$t_p =ft_s+(1-f)t_s/p$$ 。

由Gustafson的假设前提条件，总的并行执行时间为常数，假设 $$t_p = s +r = ft_s+(1-f)t_s/p =1$$ ，对该公式做一些代数变换，可以得到：$$ft_s+(1-f)t_s = p + (1-p)ft_s$$

这样就可得到加速因子如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel10.png" width="380">
</p>

这就是Gustafson定律，由它的另外一个前提，串行部分的执行时间是规定的，因此$$ft_s$$就是常数。假设常数$$K=ft_s$$，那么上式可以写成以下形式：S(p)=p+(1-p)K=K+(1-K)p。由此不难看出，加速比 和处理器个数 p 呈线性增长关系。比如说K=0.01，那么在1024个处理器上所能达到的加速比为S(1024) = 0.01 + (1-0.01)×1024 = 1013.77。下图给出了几种不同K（图片来自维基百科，其中的a就是我们所说的K，x就是p）的取值情况下的加速比和处理器个数的关系图：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel11.png" width="450">
</p>

或者我们也可以从另外一个角度来解释Gustafson定律。设想一个使用p个处理器来计算的并行程序，其总执行时间由两部分组成，即串行部分 s 和并行部分 r，为了代数上的方便，我们令s + r = 1。单独一个处理器的执行时间将是  s + pr，因此加速因子（speedup factor）就变成：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel12.png" width="300">
</p>

从图中也可以看出，在K值固定的情况下，加速比和处理器个数是呈线性增长的。Gustafson定律描述了增加处理器数目的同时相应的增大问题规模对加速比的影响，Gustafson定律认为此时加速比是线性的（执行时间不增加），实际上这种情况只在解决问题的时间和规模之间存在线性关系的时候成立，如果其关系非线性，那么就不成立。

-------------------------------------
### 参考文献与推荐阅读：

【1】 Dr Aaron Harwood，The University of Melbourne 并行与多核计算课程材料<br>
【2】 周伟明，多核计算与程序设计，华中科技大学出版社，2009年

<span style="color:blue">**（本文完）**</span>

<script>
MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
</script>
