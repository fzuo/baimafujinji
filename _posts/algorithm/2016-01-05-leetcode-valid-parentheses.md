---
layout: post
title: 括号匹配问题与经典笔试面试题目
category: 算法与结构
tags: LeetCode
keywords: LeetCode,括号匹配问题
---

### 括号匹配问题

程序设计语言是用来编写程序的工具。计算机硬件可以识别的语言是机器语言，通常人们编写的计算机程序所用的高级语言必须被转换成机器语言才可以被计算机理解。将高级语言编写的源程序翻译为等价的计算机可以理解的程序的过程称为编译，而实现编译功能的程序称为编译程序。编译程序是计算机中非常复杂的一种系统程序。在通常的集成开发环境中都，编译程序都是最精细、最复杂、也最重要的部分。

一个典型的编译程序一般都含有八个部分，即词法分析、语法分析、语义分析、中间代码生成、代码优化程序、目标代码生成、错误检查和处理以及各种信息表格的管理。它们相互协同配合完成编译任务。其中在语法分析阶段，语法分析程序以词法分析程序所输出的用内部编码格式表示的单词序列作为输入，其任务是分析源程序的结构，判断它是否为相应程序设计语言中的一个合法程序，即判定程序的合法性。

本题要求编写一个程序，该程序能够对输入的一段字符串进行括号匹配检查。若语句中左括号与右括号的数目相等，且能够完整地匹配成对，即表示本语句不存在括号匹配上的错误，此时输出“OK！”字样。否则表示语句中存在语法错误，程序输出“Wrong！”字样。显然，<span style="color:orange">这是用“栈”这种数据结构来解决的一道典型问题</span>。它的特点可以简单的概括为<span style="color:orange">“后进先出”</span>。如果把握住了这一特点，那么就是抓住了解题的关键。

下面几条语句是存在括号匹配错误的例子：

```
)(
((())
```

下面几条语句是不存在括号匹配错误的例子：

```
()
(a)((b)((c)(d)))
```

下面给出程序实现的源代码。

```cpp
#include "stdafx.h"
#include <iostream>
#include "LinkStack.h"

using namespace std;

int _tmain(int argc, _TCHAR* argv[])
{
	cout<<"请输入括号序列(以0结束):";
	LinkStack<char> small;
	char a;
	do{
		cin>>a;
		switch(a){
		case '(':small.Push(a);break;
		case ')':
			if(!small.IsEmpty()){
				small.Pop();
				break;
			}
			if(small.IsEmpty()){
				cout<<"Wrong!"<<endl;
				system("PAUSE");
				return 0;
			}
		}
	}while(a!='0');
	if(small.IsEmpty())
		cout<<"OK!"<<endl;
	else
		cout<<"Wrong!"<<endl;
	
	system("PAUSE");
	return 0;
}
```

完成编码后，编译并运行程序即可。

--------------------------------

以上内容摘选自在我的新书<span style="color:blue">**《算法之美——隐匿在数据结构背后的原理（C++版）》**</span>。探秘算法世界，求索数据结构之道；汇集经典问题，畅享编程技法之趣；点拨求职热点，敲开业界名企之门。获取更多关于算法的精彩内容，欢迎关注或参阅《算法之美（隐匿在数据结构背后的原理）》一书。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode06.png" width="190">
</p>

---------------------------------

### 两道经典面试题解析

Leetcode 是一个美国的在线编程网站，上面主要收集了各大IT公司的笔试面试题，对于应届毕业生找工作是一个不可多得的好帮手。我相信很多有求职需求的读者都刷过Leetcode上面的题目。然而，在我写作《算法之美》一书时，我还不知道这个网站的存在。说来也巧，最近有朋友推荐我看下Leetcode网站。我居然发现了上面很多题目其实和我书中所举的例子非常类似。我前面也已经push了几篇博文，介绍了《算法之美》中例题的解答，以及与之相类似的Leetcode题型。下面这道Valid Parentheses题目显然就是上面括号匹配问题的变种。先来看一下原题：

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode11.png" width="550">
</p>

这里与前面例题的主要差别主要是引入了\[ \]和{ }，但利用栈结构来解决仍然是非常容易的。而且，尽管题目没有明确地表明，但我们应该知道类似{ \[ ( ) \] }这样的形式也是正确的括号匹配模式。示例代码如下：

```c
class Solution {
public:
    bool isValid(string s) {
                stack<char> charStack; 
        int i = 0;
        while(i != s.length()){
        	
        	if (s[i] != ')' && s[i] != '}' && s[i] != ']')
                charStack.push(s[i]);  
        	else{
        		if (charStack.empty())	return false;
        		char c = charStack.top();
				charStack.pop();
				if ((c == '(' && s[i] != ')') || (c == '[' && s[i] != ']') || (c == '{' && s[i] != '}'))
					return false;
			}
        	i++;
		}
        
        if (charStack.size() == 0)  
           return true;  
        else return false;  
    }
};
```

Leetcode中另外还提供了一道与此类似的变型题，我们应当举一反三，趁热打铁，一举攻克所有该类型的题目。如下，此时我们不再要求判定输入字符串的括号匹配是否正确，而是要检测其中最长的匹配系列的长度。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode12.png" width="550">
</p>

从Leetcode上所给的提示（图中红框标出）可以看出，该题需要用到动态规划策略来解决。而且Leetcode给出的题目难度等级为Hard。事实上，Leetcode上所有涉及动态规划的问题都是Hard级别的！但是，如果我们有捷径，当然不必墨守成规。这道题其实有个非常轻巧的解决办法，用一个bool数组来标记已经匹配过的字符，找到最长的连续标记的长度就是所求的结果。采用该种方法，我们只要遍历两遍数组即可，时间复杂度为O(n)。而采用动态规划算法，复杂度则是O(n^2)。既然我们的方法更易于实现（和理解），复杂度又更低，何乐而不为？下面是示例代码。

```c
class Solution {
public:
    int longestValidParentheses(string s) {
        vector <bool> a(s.length(), false);	
        stack<int> st; 
        
        int i = 0;
        while(i != s.length()){
        	
        	if (s[i] == '(' )
                st.push(i); 
        	else if (!st.empty() && s[i] == ')'){
        		
        		a[i] = true;
				a[st.top()] = true;
				st.pop();
			}
        	i++;
		}
        
		int max_len = 0, cur_len = 0;
	    i = 0;
		while(i != s.length()){
			if (a[i]) cur_len++;
			else cur_len = 0;
			max_len = max(max_len, cur_len);
			i++;
		}
		return max_len;
    }
};
```

需要说明的是，我们使用了一个vector来储存bool数组。如果你用new来动态生成一个数组，因为数组访问是基于指针的，所以程序的执行效率会进一步提升，但是注意别忘了用delete来释放内存。

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
