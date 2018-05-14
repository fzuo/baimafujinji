
---
layout: post
title: LeetCode中的动态规划题目(2)
category: 算法与结构
tags: LeetCode
keywords: LeetCode,动态规划
---

动态规划是一种非常重要的算法设计思想。历史上有很多著名的算法都是基于这种思想设计而来的，例如：Needleman–Wunsch算法、CYK算法、FFT算法、维特比算法等等。动态规划的核心思想有两个：首先是将一个大问题拆解为若干子问题；其次是将曾经计算过的结果储存起来以备多次使用。

我们在之前的博文中已经介绍了动态规划问题的一般“套路”。并采用这种套路解决了Unique Paths 和Unique Paths II这两个问题。本文将以LeetCode中的 #64题（Min Path Sum）为例，继续运用之前介绍的套路来设计动态规划算法。问题的描述如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode03.png" width="550">
</p>

这个问题基本属于之前讨论的Unique Paths 和Unique Paths II这两个问题的变种。根据之前一样的思路，我们首先还是写出递归求解的实现（非动规版）：

```cpp
class Solution {
public:
    
    int recursivePath(int x, int y, vector<vector<int>>& grid){
        
        if(x==0 && y==0)
            return grid[0][0];
        
        if(x==0)
            return recursivePath(x, y-1, grid)+grid[x][y];
        
        if(y==0)
            return recursivePath(x-1, y, grid)+grid[x][y];
        
        int min_value = recursivePath(x-1, y, grid)<recursivePath(x, y-1, grid)?
                        recursivePath(x-1, y, grid):recursivePath(x, y-1, grid);
       
        return min_value+grid[x][y];
        
    }
    
    int minPathSum(vector<vector<int>>& grid) {
        
        int m = grid.size();
        if(m==0) return 0;
        int n = grid[0].size();
        if(n==0) return 0;
        
        return recursivePath(m-1, n-1, grid);
    }
};
```

易见，上面的实现跟之前讨论的例题唯一不同的地方仅仅在于返回值的部分，我们在这里是求了一个min计算，所以理解起来应该没有障碍。但是，上面这个版本直接提交LeetCode，会显示超时！因为其中会存在大量的重复计算。为此，需要使用解决动态规划问题的套路中之第二个步骤：定义一个数组或矩阵来存储已经得到的结果。当再次需要使用它们时，直接查表，而不用重新计算。所以，最终的实现代码如下：

```cpp
class Solution {
public:
    
    int ** matrix;
    
    int recursivePath(int x, int y, vector<vector<int>>& grid){
        
        if (matrix[x][y] != -1)
            return matrix[x][y];
        
        if(x==0)
        {
            matrix[x][y] = recursivePath(x, y-1, grid)+grid[x][y];
            return matrix[x][y];
        }
        
        if(y==0)
        {
            matrix[x][y] = recursivePath(x-1, y, grid)+grid[x][y];
            return matrix[x][y];
        }
        
        matrix[x][y] = (recursivePath(x-1, y, grid)<recursivePath(x, y-1, grid)?
                        recursivePath(x-1, y, grid):recursivePath(x, y-1, grid))+grid[x][y];
        
        return matrix[x][y];
        
    }
    
    int minPathSum(vector<vector<int>>& grid) {
        
        int m = grid.size();
        if(m==0) return 0;
        int n = grid[0].size();
        if(n==0) return 0;
        
        matrix = new int*[m];
        
        for(int i = 0; i < m; i++)
        {
            matrix[i] = new int[n];
            for(int j = 0; j < n; j++)
            {
                matrix[i][j] = -1;
            } 
        }
        
        matrix[0][0] = grid[0][0];
        
        return recursivePath(m-1, n-1, grid);
    }
};
```

上述代码可以满足题目要求。另外注意到，上述实现中我们略去了“内存回收”的部分代码。当然，由于为了演示“动态规划”的套路，这里所采用的是一种非常朴素的实现方式、并为对其进行优化，你还可以改进它，以实现更高的效率。

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
  
  
