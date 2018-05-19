---
layout: post
title: Brent原理（Brent’s Principle）
category: 多核与并发
tags: Brent原理
keywords: Brent原理
---

> 在将一个问题的内在并行性(inherent parallelism)转化为一个有效的并行算法时，Brent原理（Brent’s Principle）为这种并行化提供了一个普遍的模式。该结论最初由Richard P. Brent于1974年给出。

`Brent’s Principle：`考虑一个计算（或算法）C，在提供足够多的处理器资源之前提下（并忽略操作之间用于交互的耗时），算法C能够在PRAM上以时间 t 完成（也就是需要t个并行的步骤），假设$$m_i$$表示第 i 个步骤涉及到的操作数量，并且令

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel06.png" width="160">
</p>

那么当给定 p 个处理器时，并且要求 $$p\leq max_i m_i$$，（相同计算量的）算法C可以在时间 $$T_p$$ 内（即$$T_p$$个步骤内）完成，而且$$T_p$$小于等于 $$t+(m_i-t)/p$$ 时间完成。

**证明：**直觉上，如果第 i 个步骤需要执行 $$m_i$$ 个操作，当有 p 个处理器时，那么它可以在如下时间内被模拟

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel07.png" width="45">
</p>

而且上式必然满足：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel08.png" width="160">
</p>

既然共有t个并行的步骤，所以把它们加和，可得

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel09.png" width="350">
</p>

结论得证。

注意文献【2】中所给的形式略有不同，因为在第一个不等式处，它所采用的上限稍有修改。但是二者之结论都是统一的。


-------------------------------------
### 参考文献及推荐阅读材料：

【1】 Dr Aaron Harwood，The University of Melbourne 并行与多核计算课程材料<br>
【2】 John E. Savage，Models of Computation：Exploring the Power of Computing，Chapter 7, Brown University<br>
【3】 R. P. Brent, “The Parallel Evaluation of General Arithmetic Expressions,” JACM 21 (1974), pp:201–206.<br>

<span style="color:blue">**（本文完）**</span>

<script>
MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
</script>
