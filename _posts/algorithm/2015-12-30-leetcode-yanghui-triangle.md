---
layout: post
title: 杨辉三角与经典笔试面试题目
category: 算法与结构
tags: LeetCode
keywords: LeetCode,杨辉三角
---

### 杨辉三角问题

计算牛顿二项式系数的一个有效的方法是利用杨辉三角。杨辉三角的构造方式是将三角形每一行两边的元素置为1，其它元素为这个元素“肩”上两元素之和。该三角曾经在我国宋朝数学家杨辉1216年所著的《详解九章算法》中出现过。杨辉当时是摘录自北宋时期数学家贾宪的《开方作法本源》一书中的“开方作法本源图”。因而，人们把这个表又称为贾宪三角。在欧洲这个表被称为帕斯卡三角形。帕斯卡是1654年发现这一规律的，比杨辉迟393年，比贾宪迟600年。一个简单的五阶杨辉三角如下所示

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode13.png" width="250">
</p>

现在请利用队列实现杨辉三角的构造。输入杨辉三角的行号，程序打印相应行的二项式系数。为此，先建立一个队列并初始化队列中的元素为1，1这个队列将用来迭代生成任意行的牛顿二项式系数。接下来根据用户输入的行号，程序决定循环次数。在这些循环中，程序根据杨辉三角实际的构造情况模拟构造过程：每次形成一个新的二项式系数序列，并将这一序列保存在一个新的队列中。本次循环结束时，这个新构造的序列将作为下次循环构造另一个二项式序列的参照序列。由于LinkQueue类中并没有提供链表类的赋值方式，所以程序中要另外编写代码实现队列的赋值功能。下面给出程序实现的源代码。

```cpp
#include "stdafx.h"
#include "LinkQueue.h"
#include <iostream>

using namespace std;

template<class T>
void evaluate(LinkQueue<T>& ori,LinkQueue<T>& target){
	ori.MakeEmpty();
	while(!target.IsEmpty()){
		int tmp_value = target.DelQueue();
		ori.EnQueue(tmp_value);
	}
}

int _tmain(int argc, _TCHAR* argv[])
{
	cout<<"请输入杨辉三角阶数i(i>2):";
	int num;
	cin>>num;
	LinkQueue<int> ori;

	int ini_value = 1;
	ori.EnQueue(ini_value);
	ori.EnQueue(ini_value);
	LinkQueue<int> next;
	for(int i=0;i<num-2;i++){
		next.EnQueue(ini_value);
		while(!ori.IsEmpty()){
			int i=ori.DelQueue();
			if(!ori.IsEmpty())
			{
				int tmp = i+ori.GetFront();
				next.EnQueue(tmp);
			}
			if(ori.IsEmpty())
				next.EnQueue(i);
		}
		evaluate(ori,next);
	}
	cout<<"杨辉三角第"<<num<<"行内容如下:"<<endl;
	while(!ori.IsEmpty()){
		cout<<ori.DelQueue()<<" ";
	}
	cout<<endl;

	system("PAUSE");
	return 0;
}
```

请读者完成编码后编译运行程序，并观察输出结果。

---------------------------------

以上内容摘选自在我的新书<span style="color:blue">**《算法之美——隐匿在数据结构背后的原理（C++版）》**</span>。探秘算法世界，求索数据结构之道；汇集经典问题，畅享编程技法之趣；点拨求职热点，敲开业界名企之门。获取更多关于算法的精彩内容，欢迎关注或参阅《算法之美（隐匿在数据结构背后的原理）》一书。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode06.png" width="190">
</p>

---------------------------------

### 两道经典面试题解析

Leetcode 是一个美国的在线编程网站，上面主要收集了各大IT公司的笔试面试题，对于应届毕业生找工作是一个不可多得的好帮手。我相信很多有求职需求的读者都刷过Leetcode上面的题目。不过，因为我个人并没有求职的需求，可能也离这个时期太遥远，所以之前一直不知道这个网站的存在，实在孤陋寡闻 :（。

在《算法之美》上市之前，有朋友推荐我看下Leetcode网站。之所以会想到把杨辉三角问题拉出来讲一讲，也是因为刚好看到Leetcode上面也给出了两道类似的和杨辉三角有关的问题。当然二者也还是有区别的，在我的书里是用队列结构来求解该问题，而Leetcode上（如果选择C++语言实现）则要求基于STL中的vector来实现。先给出题目如下：

基本上Leetcode是将其分类为难度系数Easy类的题目，基本上感觉题目难度确实不大，但是如果选择C++作为实现语言，那么需要对STL中的vector有所了解。现在，特与各位分享一下我的解答（仅仅实现了功能，并未做特殊的性能优化，但已经可以通过Leetcode的测试）。代码若有欠缺之处，还望高手赐教（C++实现）。

类似的，Leetcode上还给出了另外一道与杨辉三角有关的上述题型的变种（打出完整的三角），如下：



这个网站的的好处在于它会告诉你测试数据以及你的输出和正确的输出是什么，方便大家调试学习。目前，支持C、C++、Java、Python、Ruby等多种语言。另外它是支持在线编辑，还提供了一个在线运行环境，可以直接看到运行结果。对于有兴趣刷题目或者正在求职过程中的同学，我也推荐这个资源！


<span style="color:blue">**（本文完）**</span>

------------------------------
- 本博客中已经讨论过的LeetCode题目列表
  + LeetCode中的两道动态规划题目（#62、#63）
  + LeetCode中的动态规划题目解答（2）(#64)
  + LeetCode中的动态规划题目解答（3）（#72、#718）
  + 最大连续子序列和问题（#53）
  + 看看你是否真正掌握了Binary Search(#35)
  + ZigZag排列问题与经典笔试面试题目解析（#6）
  + 括号匹配问题与经典笔试面试题目解析（#20、#32） 
  + 牛顿迭代法与一道经典编程问题（#69）
  + 三道tricky的Leetcode面试题目解析（#48、#169、#231）
  + 道关于哈希的Leetcode题目解析（#187、#389）
  + 杨辉三角与一道经典笔试面试题目（#118、#119）
  + 波兰表达式（Reverse Polish Notation）（#150）
  
  
