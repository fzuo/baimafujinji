
---
layout: post
title: 在R中绘制折线图的方法与进阶
category: 技巧与工具
tags: R语言绘图
keywords: R,绘图
---

在显示随时间而变化的连续数据时通常可以使用折线图。尤其是显示在相等时间间隔下数据的趋势时折线图则更加优势。在折线图中，类别数据沿水平轴均匀分布，所有值数据沿垂直轴均匀分布。一般分类标签是文本并且代表均匀分布且递增的数值（例如月度、季度或财年等）。

事实上，在Excel中绘制折线图是相当方便的。但我们现在要来讨论的是在R中如何绘制折线图，尤其是一些不常见但非常有用的技巧我们也会在本文中讨论。

我们首先来讨论最简单的情况，绘制一条折线：

```r
> dose = c(20, 30, 40, 50, 60)  
> drugA= c(16, 20, 27, 40, 60)  
> drugB= c(15, 18, 25, 31, 40)  
> plot(dose, drugA, type = "b") 
```

其中 type = "b" 表示同时画出点和线。上述代码执行结果如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/rplot02.png" width="360">
</p>

当使用 type = "c" 时没有点而只画出线。如果想把上图中的圆圈换成实心三角形，并想用虚线代替实现来连接各个数据点的话，则可以像下面这个做：




```r
opar <- par(no.readonly=TRUE)  #保存当前图形界面的参数
par(mfrow=c(1,3))	#1列3行

#绘制第一张图，位于左侧
plot(dose,drugA,type="b",pch=2,lty=2,col="red",ylim=c(0,max(drugA)*1.1),
      xlab="x",ylab="y",main="Example one")

#绘制第二张图，位于中间
plot(dose,drugB,type="b",pch=3,lty=3,col="blue",ylim=c(0,max(drugA)*1.1),
xlab="x",ylab="y",main="Example two")
minor.tick(nx=5,ny=2,tick.ratio=0.5)#添加次要刻度线
abline(h=c(15,30,45),lty=2,col="black")#添加参考线

#绘制第三张图，位于右侧
plot(dose,drugA,type="b",pch=2,lty=2,col="red",ylim=c(0,max(drugA)*1.1),
xlab="x",ylab="y",main="Example Three")

lines(dose,drugB,type="b",pch=17,lty=3,col="blue") #在同一幅图中同时展示两组数据 
minor.tick(nx=5,ny=2,tick.ratio=0.5)

abline(h=seq(30,50,10),lty=2,lwd=1.5,col="gray")
#添加标签
legend(locator(1), title="drug type", cex=0.75, pt.cex = 1.25, legend = c("A","B"), 
+ lty=c(2,3), pch=c(2,17),col=c("red","blue"))

par(opar)#恢复开始的图形设置
```

执行上述代码所绘制之图形如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/excel/rplot02.png" width="360">
</p>

**（本文完）**

