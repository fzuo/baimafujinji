---
layout: post
title: Flynn分类法与Schwartz的并行机类别
category: 多核与并发
tags: 并行计算分类
keywords: Flynn分类法,Schwartz并行机
---

Michael J. Flynn是美国斯坦福大学的计算机教授，1972年他提出了著名的费林分类法（Flynn's taxonomy，或Flynn’s classifications）。Flynn's taxonomy是一种经典的计算机体系结构分类方式。Flynn根据指令流、数据流的多倍性特征把计算机系统（或体系结构）分成了四类：

- SISD : single instruction single data,
- SIMD : single instruction multiple data,
- MISD : multiple instruction single data,
- MIMD : multiple instruction multiple data。

这其实也为并行计算的模型提供了四种实现方式（当前其中SISD其实就是串行的）。下图给出了SISD的抽象示意图，其中PE是precessing element，M表示Memory：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel02.png" width="500">
</p>

对于涉及大量并行数据的应用而言，SIMD架构的机器无疑是性价比最高的选择。在这种机器上，单个的control unit会将 instructions 向多个 processing elements 并行地进行广播，其中每个PE都是拥有本地存储器的一个功能单元集合。下图是SIMD的抽象示意图：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel03.png" width="500">
</p>

现在市场上的多处理系统更多属于MIMD架构的，它比SIMD更进一步。一些标准的处理器和存储芯片通过高速总线在内部进行连接（memory通常是交织的）。下图是MIMD的抽象示意图：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel04.png" width="500">
</p>

美国计算机科学家、纽约大学教授Jacob T. Schwartz定义了两种大致的方法来组织处理器和内存，即Paracomputers和Ultracomputers。Paracomputers将memory从处理中分离出来。Memory是共享的，处理器之间通过共享的memory来进行通讯。在Paracomputers中，如果我们认为每一个内存地址都可以被每一个处理器均等地访问，那么就可以据此认为PRAM模块以这种方式来被Paracomputers近似地模拟。另一方面，Ultracomputers将memory分配给若干个处理器，这样就形成了众多modules，一个处理器可以用恒定时间来访问它的module上的memory，但是访问远程的module上的memory则要耗费更长时间。

在更多的资料中，我们常常用另外两个术语来替代Paracomputers和Ultracomputers，即多处理器共享内存（multiprocessor shared memory）和多计算机分布内存（multicomputer distributed memory），如下图所示：

<p align="center">
<img src="https://fzuo.github.io/assets/img/parallel/parallel05.png" width="500">
</p>

在共享内存系统中，处理器通过对全局皆知的内存地址（globally known memory address）进行读写来实现通信。硬件实现上会确保所有处理器对内存都有用相同的访问方式（即使用单一地址空间）。因此，这也被称为是对称多处理器机“symmetric multiprocessor (SMP) machine”。而在分布内存系统中，处理器之间通过发送消息的方式来进行通信。此时硬件将仅负责消息的传递。也不再有单一地址空间。所以这也被称为是消息传送机器（message passing machine）。一个分布共享内存 (DSM) 系统拥有一个分布式的内存但是是一个单一的地址空间。 通常在SMP机器上编程要比再 message passing machine上编程容易很多。然而，message passing machine可以用更低的代价来进行扩容。

<span style="color:blue">**（本文完）**</span>
