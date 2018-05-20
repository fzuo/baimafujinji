---
layout: post
title: 求圆周率的算法：蒙特卡洛实现
category: 算法与结构
tags: Algorithm
keywords: 蒙特卡洛法,圆周率
---



什么是算法？简单地说，算法就是有穷规则构成的用于解决某一类问题的运算序列或执行步骤。在《算法之美：隐匿在数据结构背后的原理》第1章中我们讲到要解决一个问题可能会有不同的方法，当时所举的例子就是求圆周率π的近似值。对于这个问题你能想到多少种算法呢？

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode06.png" width="190">
</p>

探秘算法世界，求索数据结构之道；汇集经典问题，畅享编程技法之趣；点拨求职热点，敲开业界名企之门。

本书内容简介及勘误表请参见《算法之美隆重上市欢迎关注（另附勘误表在此）》。


方法(1)：首先根据微积分中的泰勒公式，可以使用下面这个公式来求π的近似值

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program13.png" width="450">
</p>

方法(2)：其次可以还可以利用下面这个马青公式来求解π的近似值：

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program14.png" width="410">
</p>

马青公式由英国天文学家约翰·马青（John Machin，1686 –1751）于1706年发现，他利用这个公式计算到了100位的圆周率。此外，上式中的反三角函数可以领用下面这个泰来展开式进行计算：

<p align="center">
<img src="https://fzuo.github.io/assets/img/program/program15.png" width="430">
</p>

方法(3)：最后，也是我们准备来动手实践一下的就是利用蒙特卡洛法。这个方法背后的理论基础是概率论中的伯努利大数定律。众所周知，圆的面积公式为 $$S = πr^2$$，当 r = 1 时，S = π。如果在一个边长为1的正方形中有一个内切圆，那么该圆的面积就是π/4。利用这一原理，可以假设有大量的随机点等概率均匀地落入此正方形中。若正方形的面积为S'，内切圆的面积为S，如果落入正方形中的点数为 N'，落入内切圆的面积为N，这时便有 S'/S = N'/N，所以可据此求得 π = 4×N/N'。

下面就编写一个Java程序来实现蒙特卡洛法求π的近似值：

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class MonteCarloPi {

	public static void main(String[] args) throws Exception{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		System.out.print("How many darts shoud I toss at the board?\n");
		String s = br.readLine();
		int numberOfDarts = Integer.parseInt(s.trim());
		double radius = 1.0;
		Dartboard d = new Dartboard(radius);
		
		for(int i=1; i<=numberOfDarts; i++){
			Toss t = Toss.getRandom(radius);
			d.strike(t);
		}
		
		double fractionIn = d.getFractionIn();
		double pi = 4.0 * fractionIn;
		System.out.println("Pi is approximately "+pi);
	}

}

class Dartboard{
	
	private double radius;
	private int insideCircle, outsideCircle;
	
	public Dartboard(double radius){
		this.radius = radius;
		insideCircle = 0;
		outsideCircle = 0;
	}
	
	public void strike(Toss toss){
		double x = toss.getX();
		double y = toss.getY();
		
		if(Math.sqrt(x*x + y*y) < radius)
			insideCircle++;
		else
			outsideCircle++;
	}
	
	public double getFractionIn() {
		double total = (double) (insideCircle + outsideCircle);
		return (double) insideCircle/total;
	}
}

class Toss{
	
	private double x,y;
	
	public Toss(double x, double y){
		this.x = x;
		this.y = y;
	}
	public double getX(){return x;}
	public double getY(){return y;}
	
	public static Toss getRandom(double radius){
		
		double x,y;
		double size = Math.random();
		double sign = Math.random();
		size = size * radius;
		
		if(sign > 0.5)
			x = size;
		else
			x = -size;
		
		size = Math.random();
		sign = Math.random();
		size = size * radius;
		
		if(sign > 0.5)
			y = size;
		else
			y = -size;
		
		return new Toss(x,y);
	}
}
```

执行一下上述代码，然后我们来验证一下其效果如何...

```
How many darts shoud I toss at the board?
1000000
Pi is approximately 3.142028
```

可见所得到的结果较好地近似于圆周率 π 。

<span style="color:blue">**（本文完）**</span>
