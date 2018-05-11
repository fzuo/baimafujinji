---
layout: post
title: R语言绘制坐标轴与线段
category: 技巧与工具
tags: R语言绘图
keywords: R,绘图
---

R语言绘图的能力非常强大，有时看似繁复的图形不过寥寥数行代码就搞定。但是其中确实有一些小技巧平时并不太容易引起注意。通常情况下我们画出的图形周围会有一个方框。但是如果我们想画出坐标轴（横轴以及竖轴）的话那该怎么办呢？例如，下面这张图该如何来绘制的？

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/rplot01.png" width="360">
</p>

首先，如果你不想要原图外面的那个框，那么相应绘图参数中的 axes 应被置为FALSE。此外，重新定制的坐标轴，需要使用函数arrows()。示例代码如下：

```r
curve(x^2-2, col = "blue", xlim = c(1,3.2), ylim = c(-1, 7), axes = FALSE, xlab = '', ylab = '')
arrows(0, 0, 3.2, 0)

y1 <- function(x) {6*x-11}
y2 <- function(x) {(11/3)*x-(242-49)/36}
curve(y1, add = T, col = "red", lty = 2, from=11/6)
curve(y2, add = T, col = "red", lty = 2, from=(242-49)/132 , to=2.6)

par(new=TRUE)

xs = c(3, 3, 11/6, 11/6,  (242-49)/132, (242-49)/132)
ys = c(0, 7, 0, 49/36, 0, ((242-49)/132)^2-2)
plot(xs, ys, col="green", xlim=c(1,3.2), ylim = c(-1, 7), axes = FALSE, xlab = '', ylab = '')

s <- c(1,3,5)
segments(xs[s], ys[s], xs[s+1], ys[s+1], col= 'green', lty = 3)
```

此外，上面代码中最后绘制绿色虚线段的方法也很有参考价值。

