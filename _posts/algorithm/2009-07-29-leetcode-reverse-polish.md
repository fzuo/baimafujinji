---
layout: post
title: 逆波兰表达式
category: 算法与结构
tags: LeetCode
keywords: LeetCode,逆波兰表达式
---


> 逆波兰表示法（RPN，Reverse Polish Notation），是一种是由波兰逻辑学家与哲学家扬·武卡谢维奇（Jan Łukasiewicz）1920年引入的数学表达式方式，在逆波兰记法中，所有操作符置于操作数的后面，因此也被称为后缀表示法。逆波兰结构由弗里德里希·鲍尔（Friedrich L. Bauer）和艾兹格·迪科斯彻在1960年代早期提议用于表达式求值，以利用堆栈结构和减少计算机内存访问。

逆波兰记法中，操作符置于操作数的后面。例如表达“三加四”时，写作“3 4 +”，而不是“3 + 4”。如果有多个操作符，操作符置于第二个操作数的后面，所以常规中缀记法的“3 - 4 + 5”在逆波兰记法中写作“3 4 - 5 +”：先3减去4，再加上5。使用逆波兰记法的一个好处是不需要使用括号来标识操作符的优先级。例如中缀记法中“3 - 4 × 5”与“（3 - 4）×5”不相同，但后缀记法中前者写做“3 4 5 × -”，无歧义地表示“3 (4 5 ×) −”；后者写做“3 4 - 5 ×”。

逆波兰表达式的解释器一般是基于堆栈的。解释过程一般是：操作数入栈；遇到操作符时，操作数出栈，求值，将结果入栈；当一遍后，栈顶就是表达式的值。因此逆波兰表达式的求值使用堆栈结构很容易实现，和能很快求值。

来看一道Leetcode算法题目。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode16.png" width="550">
</p>

下面给出C++下的实现代码：

```cpp
class Solution {
public:
    
    int evalRPN(vector<string> &tokens) {
        unsigned long size = tokens.size();
        int num1 , num2;
        stack<int> st;
        
        for(int i = 0 ; i < size ; i++){
            string token = tokens[i];
            if((token[0] == '+' || token[0] == '-' ||
                token[0] == '*' || token[0] == '/') && token.length() == 1){
                
                num1 = st.top() ; st.pop();
                num2 = st.top() ; st.pop();
                switch(token[0]){
                    case '+' : st.push(num2 + num1);break;
                    case '-' : st.push(num2 - num1);break;
                    case '*' : st.push(num2 * num1);break;
                    case '/' : st.push(num2 / num1);break;
                }
                
            }else{
                st.push(atoi(token.c_str()));
            }
        }
        return st.top();
    }
};

```

同时，我们也给出一个测试用的主函数：

```cpp
#include <iostream>
#include <stack>
#include <vector>
#include <string>

using namespace std;

int main(int argc, const char * argv[]) {
   
    vector<string> str2;
    str2.push_back("4");
    str2.push_back("13");
    str2.push_back("5");
    str2.push_back("/");
    str2.push_back("+");
    
    Solution ss;
    int res = ss.evalRPN(str2);
    
    cout <<res<<endl;
    
    return 0;
}
```

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


