---
layout: post
title: 多重共线性的确定方法
category: 机器学习
tags: Regression
keywords: Regression,多重共线性
---


回归分析是数据挖掘中最基本的方法，其中基于普通最小二乘法的多元线性回归要求模型中的特征数据不能存在有多重共线性，否则模型的可信度将大打折扣。但是就是技术而言，如何确定模型中的各各特征之间是否有多重共线性呢？

先来看一组数据。根据经验知道，某市的货运量$$y$$ （单位：$$10^6$$吨）与该市工业总产值$$x_1$$ （单位：亿元）及农业总产值$$x_2$$ (单位：亿元）有关。当规划部门制定了工农业总产值目标后，需要对货运量进行预测，为此要建立y关于$$x_1$$和$$x_2$$的回归方程。现在收集了10组数据，如表11-1所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel0.png" width="450">
</p>

我们能否用这组数据来建立多元线性回归模型呢？现在这组数据的问题还不十分明显，我们不妨用最简单的交叉散点图来透视一下数据可能存在的问题。于是我们在R中绘制了下图：

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel1.png" width="520">
</p>

从散点图中我们可以看出每个解释变量都与被解释变量有较明显的线性关系，而且这也是我们所希望看到的。但是两个解释变量之间似乎也呈现出了某种线性关系。事实上，如果计算两个解释变量之间的相关性系数，可知它们的线性相关系数很高，这也就意味着严重的多重共线性。在构建多元线性回归模型时，随着解释变量数目的增多，其中某两个解释变量之间产生多重共线性是很容易发生的情况。此时就需要考虑是否将其中某个变量从模型中剔除出去，甚至是重新考虑模型的构建。

但是这种视觉观察的方法仅仅能够作为定性研究的一种前导，最好能够有一种定量的方法来帮助我们最终抉择。为此再来看一组数据。根据经验知道，沉淀物吸收能力是土壤的一项重要特征，因为它会影响杀虫剂和其他各种农药的有效性。在一项实验中，我们测定了若干组土壤样本的如下一些情况，数据如表2所示。其中，$$y$$表示磷酸盐吸收指标：$$x_1$$和$$x_2$$分别表示可提取的铁含量与可提取的铝含量。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel2.png" width="450">
</p>

我们能否用这组数据来建立多元线性回归模型呢？同样再来绘制散点图如下，自变量之间似乎还是有点共线性，但是又不像上面例子中的那么明显，现在该怎么办？

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel3.png" width="520">
</p>

所以我们还是建议采用一种更加能够便于量化的方法来描述问题的严重性，而不是仅仅通过肉眼观察来做感性的决定。下面我演示在SPSS 22中检验多重共线性的方法。首先导入数据，如下所示

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel4.png" width="580">
</p>

然后单击菜单栏上的【分析】->【回归】->【线性...】，则进入如下图所示的线性回归对话框。当选择好因变量和自变量之后，选择右上角的【Statistics...】，然后在弹出的新对话框里选定【共线性诊断】

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel5.png" width="580">
</p>

回到上图左边的对话框之后，选择确定，SPSS给出了线性回归分析的结果。我们来看其中共线性诊断的部分，如下所示，如果有条件指数>10，则表明有共线性。现在最大的是9.659，仍然处于可以接受的范围。

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel6.png" width="580">
</p>

如果我们使用的数据是最前面给出的那组货运量与工农业产值的例子，又将如何，来看下图。红色框所标出的条件指数高达23.973(>10)，可见共线性是确凿无疑的了！

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel7.png" width="580">
</p>

<span style="color:blue">**（本文完）**</span>

<script> MathJax.Hub.Queue(["Typeset",MathJax.Hub]); </script> 
