---
layout: post
title: 生成模型与判别模型
category: 机器学习
tags: 机器学习模型
keywords: Generative,Discriminative
---

机器学习中有很多算法，或者说是很多模型，比较流行的包括：决策树、朴素贝叶斯、贝叶斯网络、逻辑回归、支持向量机、人工神经网络、随机森林、隐马尔科夫模型、*k*NN、*k*-Means、AdaBoost、EM ... ...

如果要把它们归个类，比较常见的可以分成 “supervised 和 unsupervised” 或者 “贝叶斯派 和 频率派”等等。把为数众多的算法分成几类，绝对不仅仅是为了给算法们贴上标签，或者仅仅是为了分类而分类。事实上，当我们把某个算法看做这一类或者那一类的同时，更重要的是通过这种划分体现出算法本质上的某种特殊属性。

除了上面提到的那几种划分以外，另外一种常常被提到的分类法就是本文将要讨论的 “<span style="color:blue">Generative Models（生成模型）</span>” 和 “<span style="color:blue">Discriminative Models（判别模型）</span>” 。这种分类方法所使用的两个标签相当不容易理解，至少从字面意思上并不能很明显地体现出它们的内在逻辑。

不妨以本博客介绍过的两个经典的机器学习模型朴素贝叶斯（Naive Bayes）和逻辑回归（logistic regression）为例来探讨Generative 和 Discriminative 的区别。Logistic regression 是一个 discriminative 的分类器，而 naive Bayes 却是一个 generative 的分类器。

回想一下分类器（Classifier）的机制，当我们有一些 输入（特征向量）**x** 时，我们要为样本选择一个最合适的（分类）标签y，为此就需要求得条件概率 P(y\|**x**) 的大小，其中令整个条件概率最大的 y 就是最合适的分类结果。在朴素贝叶斯法中，我们以贝叶斯定理为基础，借助似然P(**x**\|y) 和 先验 P(y) 来”间接“地评估哪个y才是最合适的，即

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel31.png" width="320">
</p>

似然 P(**x**\|y) 的意思就是在给定类别标签y的时候，试图预测那些特征更有可能会出现（也即是会产生何种的**x**），所以我们说如果一个模型是generative model，那么这个模型就是被训练成 通过类别 y 来生成数据 **x** （a model that is trained to generate the data **x** from the class y）。然后我们再通过贝叶斯定理来计算我们想要的概率 P(y\|**x**)。

一个 discriminative  model 会采用一个直接的方法，它通过在不同的类别y的可能取值之间进行判定，从而计算出概率 P(y\|**x**) ，即 

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/excel32.png" width="150">
</p>

根据上面这种分类方式，我们可以列举一些 generative models 的例子：

- Naive Bayes
- Gaussian mixture model and other types of mixture model
- Hidden Markov model
- Probabilistic context-free grammar
- Latent Dirichlet allocation
- Restricted Boltzmann machine

同样，可以列举一些 discriminative models 的例子:

- Logistic regression
- Support vector machines
- Conditional random fields
- Neural networks

<span style="color:blue">**（本文完）**</span>

