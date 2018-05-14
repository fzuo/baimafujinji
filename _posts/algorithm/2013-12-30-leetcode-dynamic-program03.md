---
layout: post
title: LeetCode中的动态规划题目(3)
category: 算法与结构
tags: LeetCode
keywords: LeetCode,动态规划
---

动态规划是一种非常重要的算法设计思想。历史上有很多著名的算法都是基于这种思想设计而来的，例如：Needleman–Wunsch算法、CYK算法、FFT算法、维特比算法等等。动态规划的核心思想有两个：首先是将一个大问题拆解为若干子问题；其次是将曾经计算过的结果储存起来以备多次使用。

在本系列之前的文章中，我们已经介绍了动态规划算法设计的一种基本套路。但现实中的动态规划问题其实是五花八门的。之前的动规例子的解答都使用了递归，本文所补充的两个例子则采用循环。

首先是LeetCode中的#718题（Maximum Length of Repeated Subarray）。该问题的描述如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode04.png" width="550">
</p>

动态规划的一个核心思想是把之前已经得到的计算结果存储起来，以备复用。这其实基本上就是在用空间换时间。就本题而言，为了存储已经算过的结果，我们设计一个矩阵，并将其中的所有元素初始化为0。如果数组A中的元素A[i]与数组B中的元素B[j]相同，那么就更新矩阵中的一个元素[i,j]为1+[i-1,j-1]，1表示当前这个字符匹配，而[i-1,j-1]表示之前的最长匹配子串。这其实也就是动规的状态转移方程。具体的实现代码如下：

```cpp
class Solution {
public:
    int findLength(vector<int>& A, vector<int>& B) {

    	int m = A.size();
    	int n = B.size();

    	int ** matrix = new int*[m+1];
        
        for(int i = 0; i < m+1; i++)
        {
            matrix[i] = new int[n+1];
            for(int j = 0; j < n+1; j++)
            {
                	matrix[i][j] = 0;
            } 
        }
        
        int max = 0;

        for(int i = 1; i < m+1; i++)
        {
            for(int j = 1; j < n+1; j++)
            {
            	if(A[i-1]==B[j-1])
            	{
        			matrix[i][j] = 1 + matrix[i-1][j-1];
        			max = max > matrix[i][j] ? max : matrix[i][j];
            	}
            	else
                	matrix[i][j] = 0;
            } 
        }
        
        return max;
    }
};
```

上述代码可以满足题目要求。另外注意到，上述实现中我们略去了“内存回收”的部分代码。当然，由于为了演示“动态规划”的套路，这里所采用的是一种非常朴素的实现方式、并为对其进行优化，你还可以改进它，以实现更高的效率。

与上面这道题目类似的还有#72题（ Edit Distance）。该题目是要计算两个单词之间的编辑距离，其具体描述如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode05.png" width="550">
</p>

这道题目和前面的题目思路差不多，但是却复杂很多。题目本身可以使用著名的德勒曼-温施（Needleman-Wunsch）算法直接解决。Needleman-Wunsch算法也是历史上最早的应用动态规划思想设计的算法之一。关于该算法的更多内容，读者可以参考《算法之美（隐匿在数据结构背后的原理）》一书中的第3章，此处不再赘述。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode06.png" width="200">
</p>

需要说明的是，在具体使用Needleman-Wunsch算法时，要为各种编辑操作赋上不同的分值，而这个分值可以由使用者自行定义。而这道LeetCode题目的要求其实已经指明插入、删除和替换都对应于1个步骤（所以可以量化为1分）。这与书上作为例子时所设定的算分极值不同。下面我们给出最终的实现代码：

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        
        int m = word1.size();
        int n = word2.size(); 

        if(m==0)  return n;
        if(n==0)  return m;

        int ** matrix = new int*[m+1];  
        
        int i = 0;
        int j = 0;
        int p = 0;
        int q = 0;

        for(i = 0; i < m+1; i++)  
        {  
            matrix[i] = new int[n+1];
            matrix[i][0] = 0+i; 
        }

        for(j = 0; j < n+1; j++)  
        {  
            matrix[0][j] = 0+j;  
        }

        for(i = 1; i < m+1; i++)  
        {  
            for(j = 1; j < n+1; j++)  
            {  

            	p = matrix[i][j-1] + 1 < matrix[i-1][j] + 1 ? matrix[i][j-1] + 1 : matrix[i-1][j] + 1;

                if(word1[i-1]==word2[j-1])  
                {  
                    q = matrix[i-1][j-1];  
                }  
                else
                {
                	q = matrix[i-1][j-1] + 1;   
                }

                matrix[i][j] = p < q ? p : q; 
            }   
        }

        return matrix[m][n]; 
         
    }
};
```

同样，上述实现中我们略去了“内存回收”的部分代码。

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
  
