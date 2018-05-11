---
layout: post
title: R语言绘图技巧之显示希腊字母
category: 技巧与工具
tags: R语言绘图
keywords: R,绘图
---

R语言绘图的能力非常强大，有时看似繁复的图形不过寥寥数行代码就搞定。但是其中确实有一些小技巧平时并不太容易引起注意。例如下面图中的图例部分（右上角），我们引入了一些希腊字母，这些符号是如何输入的呢？

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/rplot06.png" width="420">
</p>

其实R的绘图过程中，希腊字母本身的写法有点像LaTeX（当然我们并不需要那个斜线），例如alpha, pi, mu等等。但是要让这些希腊字母正确显示，就必须要借用函数expression()，例如下面的示例代码：

