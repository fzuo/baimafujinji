---
layout: post
title: CES生产函数
category: 经济学研究
tags: Economy
keywords: 生产函数,CES
---


> 在经济学中，CES即常数替代弹性（Constant Elasticity of Substitution）是某些生产函数（production functions）或效用函数（utility functions）的重要性质，而具有该性质的生产函数就是所谓的CES生产函数。通常，我们所讨论的CES特性，都是针对二元函数而言的，当然也可以将其推广到多元函数的情形。

CES 生产函数是一个新古典生产函数，它体现了常数替代弹性。两要素的（资本 K 和劳力 L）CES 生产函数由诺贝尔经济学奖得主 Solow 提出，其形式如下

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces01.png" width="250">
</p>

在生产中，对于一个多种生产要素的生产函数而言，任两种要素之间都有一定的替代性，即当一种要素减少时，可通过增加另外一种要素作为弥补，以得到同样多的产品。例如，当机器设备比较昂贵，但相对而言工人工资比较低时，即资本成本大于劳力成本，那么可以少买些机器，而多聘请一些工人，通过这种方式来弥补由于机器较少而导致的产出减少，从而保持总产出不变。所以劳力和资本之间存在替代性。边际替代率就是生产理论中用于表示替代性的一种非常基础的概念：

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces02.png" width="110">
</p>

边际产量 ∂Y/∂L 和 ∂Y/∂K 的比叫做劳力 L 对资本 K 的边际替代率。

- ∂Y/∂L 表示劳力 L 的边际产出量，即劳力单位收益，也称工资本

- ∂Y/∂K 表示资本 K 的边际产出量，即资本的价格率，也称利润率

显然，等产量线的斜率就是我们所要考察的边际替代率，它表示 L 替代 K 的能力的大小，也就是在 Y 保持不变的条件下，增加一单位 L 可以减少多个单位的 K 。

同样可以计算资本对劳力的边际替代率，它与劳力对资本的边际替代率互为倒数。对于Cobb-Douglas生产函数 

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces03.png" width="100">
</p>

劳力 L 对资本 K 的边际替代率，

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces04.png" width="250">
</p>

可得

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces05.png" width="240">
</p>

在市场经济中，生产要素之间的替代由它们的相对价格变动所引起。例如，当工资率相对于利润率上升时，生产中减少劳动用量，增加资本用量就较为有利。因此，有人又提出用工资率对利润率之比变动 1% 引起资本对劳力之比变动百分之几来衡量资本与劳动之间的替代弹性。以二元生产函数 Y(K, L) 为例，替代弹性 σ 可表示为

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces06.png" width="140">
</p>

现在假设有如下形式的CES函数

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces07.png" width="200">
</p>

我们来试求 K 和 L 之间的替代弹性，首先求出 K 对 L 的边际替代率

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces08.png" width="200">
</p>

再求 K 和 L 之间的替代弹性

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces09.png" width="220">
</p>

可见，当参数一定时，CES函数的替代弹性就是一个常数。而且，显而易见的是，当 ρ 趋近于0时，σ 趋近于1，这时，CES函数趋近于Cobb-Douglas生产函数。要证明这个看似诡异的结论，需要用到一点数学技巧。下面我们就来审视一下它的证明细节。

首先我们要来证明下面这个结论：

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces10.png" width="130">
</p>

这是因为

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces11.png" width="180">
</p>

所以可得

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces12.png" width="250">
</p>

所以结论得证。基于这个结论我们来证明CES函数（其中，α + β = 1）

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces13.png" width="220">
</p>

和Cobb-Douglas生产函数

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces14.png" width="110">
</p>

之间的关系。首先，对于Cobb-Douglas生产函数等号两边分别取对数可得

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces15.png" width="120">
</p>

同样，对CES函数等号两边分别取对数，再取极限可得

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces16.png" width="200">
</p>

根据洛必达法则，对分式上下分布求导

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces17.png" width="250">
</p>

其中，求导部分的中间计算过程如下（主要要使用前面正面的结论）：

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces18.png" width="220">
</p>

至此，证明完毕。

最后，我们还可以把CES函数推广到多元的情形，如下

<p align="center">
<img src="https://fzuo.github.io/assets/img/production_func/ces19.png" width="150">
</p>

<span style="color:blue">**(全文完）**</span>
