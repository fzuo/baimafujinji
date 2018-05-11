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

```r
curve(dgamma(x, shape = 0.5), add = T,,col = "red", ylim=c(0, 0.5), xlim=c(0,15))
curve(dgamma(x, shape = 1), add = T, col = "orange", ylim=c(0, 0.5), xlim=c(0,15))
curve(dgamma(x, shape = 2), add = T, ylim=c(0, 0.5), col = "green", xlim=c(0,15))
curve(dgamma(x, shape = 3), add = T, ylim=c(0, 0.5), col = "pink", xlim=c(0,15))
curve(dgamma(x, shape = 6), add = T, ylim=c(0, 0.5), col = "blue", xlim=c(0,15))

text.legend = c(expression(paste(alpha, " = 0.5")),expression(paste(alpha, " = 1")), 
                expression(paste(alpha, " = 2")), expression(paste(alpha, " = 3")), 
                expression(paste(alpha, " = 6")))
legend("topright", legend = text.legend, lty = c(1,1), col = c( "red", "orange","green", "pink", "blue"))
```

上面代码除了添加希腊字母的用法之外，向图例的引入，多条曲线的叠加部分也很有参考价值。

