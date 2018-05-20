---
layout: post
title: A*寻路算法的最简单通俗讲解
category: 算法与结构
tags: Algorithm
keywords: A*寻路算法
---

<span style="color:blue">为什么我的眼里常含泪水？因为我有一个算法不会。</span>为了节约点眼泪，今天我们就来介绍著名的<span style="color:blue">A\*寻路算法</span>（或简称A\*算法）。


Peter Hart, Nils Nilsson 和 Bertram Raphael 在1968年首次提出了著名的A\*算法。A\*算法其实是Dijkstra最短路径算法的一种扩展。不一样的是，在A\*算法中我们需要使用Heuristics 来导引搜索过程，以期达到更好的表现。


问题本身跟Dijkstra最短路径算法差不多，还是要求从起点（<span style="color:red">**S**</span>tart）到终点（<span style="color:red">**G**</span>oal）的一条最短路径，然而此时我们同时还有一组与加权图中每个节点相对应的Heuristics来辅助搜索过程。每个节点（或者状态）State 都有一个H值，这个值相当于对“从该点出发到终点的距离”的一个大概的估计，唯一要注意的是这个启发值不能够大于实际值。例如，G点本来就是终点，所以它对应的H就是0。A点到G点的最短路径应该是8，所以启发值就设为一个不大于8的值，例如6。


A\*算法现在被广泛应用与电脑游戏中的路径规划问题。我们就以此为例来介绍A\*算法的具体实施步骤。如下图所示（该例子来自文献【3】），其中A表示起点，B表示终点，黑色的实心方块表示障碍物。此外我们假设水平或垂直方向上相邻的两个方块之间距离是10，那么对角线方向上相邻的两个方块距离就约是14。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program03.png" width="320">
</p>

算法开始，我们首先搜索A相邻的所有可能的移动位置（对应于图中的绿色方块）。每个方块左上角的值G表示该点到A的距离，右上角值为H，注意H不能大于该点到B的距离，所以这里的H就取其到B的距离。最后，还要计算一个F值，F=H+G。

然后像Dijkstra算法一样，我们选一个F值最小的节点来做继续搜索。也就是上图中A的邻域中位于左上角的值（F=42）。然后更新该节点的邻域值。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program05.png" width="400">
</p>

这时你会发现，出现了三个F值都等于48的节点。到底应该选择哪一个来继续接下来的搜索呢？这时需要考察它们中的那个H值最小，结果发现H=24是最小的，所以下面就要从该点出发继续搜索。于是更新该节点的邻域方块中的值。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program06.png" width="400">
</p>

这个时候再找出全局F值最小的点，结果发现有两个为48（而且它们的H值也相当），于是随机选取一个作为新的出发点并更新其邻域值（例如选择右上方的方块），然后在从全局选取F最小的更新其邻域值，于是有：

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program07.png" width="400">
</p>

此时全局F最小的值为54，而且F=54的节点有两个，所以我们还是选择其中H值最小的来做更新。于是更新该节点邻域方块中的值。这里你需要注意的一个地方是，F=54的红色节点下方邻域（F=68）的方块中，G=38。但是，从A到该节点的最短路径应该是30。这是因为目前程序所选择的路径是下图中紫色线路所规程出来的路径，其G的增长序列是14→24→38。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program08.png" width="400">
</p>

不过不要紧，只要继续执行算法，更新全局F值为最小节点（F=54）的方块，上面的G值就会给更新为正确的值了。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program09.png" width="400">
</p>

此时，全局F值最小的方块中F=60，所以更新该节点邻域方块中的值。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program10.png" width="400">
</p>

现在全局F值最小的有两个，都为68，此时先更新H最小的。这是其实程序已经发现左侧F=68的节点并不能引导一条更短的路径。于是接下来就要转向右侧F=68的节点，并以此为新起点搜索路径。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program11.png" width="400">
</p>

最终反复执行上述过程，你就会得到如下图中蓝色方块所示的一条最短路径。

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program12.png" width="400">
</p>

最后，如果读者有兴趣，不妨思考一下如果要在本文最开始给出的有向加权图中执行A\*寻路算法，其具体过程会是什么样的。

------------------------------
### 参考文献

【1】[网址链接](https://en.wikipedia.org/wiki/A*_search_algorithm)<br>
【2】[UC Berkeley人工智能课程视频](https://www.youtube.com/watch?v=sAoBeujec74)<br>
【3】Sebastian Lague讲解A\*算法的[视频](https://www.youtube.com/watch?v=-L-WgKMFuhE)<br>

<span style="color:blue">**（本文完）**</span>