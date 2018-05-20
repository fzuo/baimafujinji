---
layout: post
title: ZigZag排列问题与经典笔面试题目
category: 算法与结构
tags: LeetCode
keywords: LeetCode,ZigZag
---

### 图像压缩编码中的Z字排序

JPEG（Joint Photographic ExpertsGroup）是一种常见的图像文件格式，也是目前静态图像中压缩比最高的一种图像文件格式，它综合运用了多种压缩技术而达到一种极高的压缩比例。JPEG是作为一个国际数字图像压缩标准，压缩技术十分先进，它用有损压缩方式去除冗余的图像和彩色数据，获取得极高的压缩率的同时能展现十分丰富生动的图像。目前，它已被广泛地应用与多媒体和网络程序中。通常，在JEPG编码过程中，有一个非常重要的步骤，即Z字形编排过程。Z字形编排过程大致是这样的：经过前期处理的图像被分为若干个 的小图像块，此时就从小图像块的左上角开始沿Z字形对图像元素进行遍历，并将遍历所得的结果重新写入等大小的图像块中，整个过程如图2-15所示。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode09.png" width="500">
</p>

要实现这样一个Z字形排列可能读者咋一看会感觉无从下手。但是在分析了Z字形遍历原矩阵过程中的走向规律，其实可以设计一个非常简单的算法来实现这种编排。对于原始矩阵matrix中的任意元素matrix\[i\]\[j\]的遍历走向规律可以分为如下三种情况：

- 如果二维数组中的元素matrix\[i\]\[j\]中纵坐标j是偶数，且i=0或者i=7，那么遍历路径在矩阵中的走向就是水平向右移动一格。

- 如果二维数组中的元素matrix\[i\]\[j\]中纵坐标i是奇数，且j=0或者j=7，，那么遍历路径在矩阵中的走向就是垂直向下移动一格。

- 除上述规则以外的情况，如果二维数组中的元素matrix\[i\]\[j\]的横纵坐标和i+j是偶数，则遍历路径在矩阵中的走向就是右上角移动一格；否则，若i+j是奇数，则遍历路径在矩阵中的走向就是左下角移动一格。

基于上述原则，完成矩阵Z字形编排功能的代码清单如下。

```cpp
#include <iostream>
#include <iomanip>

using namespace std;

#define SIZE 8

int main(int argc, char** argv) {
	int matrix[SIZE][SIZE] = {0};
	int a[SIZE][SIZE] = {0};

	int i, j, x, y, value = 0;

	int * p;
	p = &matrix[0][0];
	//初始化矩阵
	for(i = 0; i<SIZE * SIZE; i++)
		*p++ = i;

	//打印原始矩阵
	cout<<"原始矩阵如下:"<<endl;
	for(i = 0; i < SIZE; i++)
	{
		for(j = 0; j < SIZE; j++)
			cout<<setw(4)<< *(*(matrix + i) + j);
		cout << endl;
	}

	i = 0; j =0;
	//进行Z字编排
	for(x = 0; x < SIZE; x++)
		for(y = 0; y < SIZE; y++)
		{
			*(*(a + i) + j) = *(*(matrix + x) + y);

			if((i == SIZE-1 || i == 0) && j%2 == 0)
			{
				j++;
				continue;
			}

			if((j == 0 || j == SIZE-1) && i%2 == 1)
			{
				i++;
				continue;	
			}

			if((i+j)%2 == 0)
			{
				i--;
				j++;
			}
			else if((i+j)%2 == 1)
			{
				i++;
				j--;
			}
		}

	cout<<endl<<"经过Z字编排后的矩阵如下:"<<endl;
	for(i = 0; i < SIZE; i++)
	{
		for(j = 0; j < SIZE; j++)
			cout<<setw(4)<< *(*(a + i) + j);
		cout<<endl;
	}
	return 0;
}
```

请读者完成编码后编译运行程序，并观察输出结果。此外，这个算法不仅对8×8的矩阵有效，对于SIZE取其他值仍然有效，读者不妨试试看。

---------------------------------

以上内容摘选自在我的新书<span style="color:blue">**《算法之美——隐匿在数据结构背后的原理（C++版）》**</span>。探秘算法世界，求索数据结构之道；汇集经典问题，畅享编程技法之趣；点拨求职热点，敲开业界名企之门。获取更多关于算法的精彩内容，欢迎关注或参阅《算法之美（隐匿在数据结构背后的原理）》一书。

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode06.png" width="190">
</p>

---------------------------------

### 一道经典面试题解析

Leetcode 是一个美国的在线编程网站，上面主要收集了各大IT公司的笔试面试题，对于应届毕业生找工作是一个不可多得的好帮手。我相信很多有求职需求的读者都刷过Leetcode上面的题目。不过，因为我个人并没有求职的需求，可能也离这个时期太遥远，所以之前一直不知道这个网站的存在，实在孤陋寡闻 :（。

上面那本书还没有上市，之所以会想到把Z字编排问题拉出来讲一讲，也是因为最近有朋友推荐我看下Leetcode网站。而且刚好看到和上面Z字编排问题非常类似的一道经典面试题目。当然二者也还是有区别的，可以将它们归为同一类型的题目。类似的，其实还可以给出其他变种题型。先给出题目如下：

<p align="center">
<img src="https://fzuo.github.io/assets/img/leetcode/leetcode10.png" width="550">
</p>

或者你可以访问[网页链接](https://leetcode.com/problems/zigzag-conversion/)。

索性我也来试做一下，还好宝刀未老，基本上Leetcode是将其分类为难度系数Easy类的题目，基本上感觉题目难度确实不大，但是还是要做到考虑全面，有一些细节上第一次做很可能考虑不到而不能通过Online 测试。现在，特与各位分享一下我的解答。代码若有欠缺之处，还望高手赐教（C++实现）。

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
    	int s_c = sizeof(char);
        int length = s.length();
        int num = 1;       
        if (numRows*2-2 != 0)
        	num = 1 + (length/(numRows*2-2));
        else 
        	num = 1 + length;
        
        char * matrix = new char[numRows*s_c*num*2];
        int index = 0;
		
		//Initialize the string
       for(int j = 0; j < numRows; j++)
			for(int i = 0; i < num*2; i++)
				matrix[j*num*2*s_c + i*s_c] = ' ';
        
        for(int i = 0; i < num*2; i++){
        	if (i%2 == 0){
			    for(int j = 0; j < numRows; j++){
			    	if(s[index] != '\0'){
			    		matrix[j*num*2*s_c + i*s_c] = s[index];
						index ++;
					}
				}
			}
			else if(i%2 == 1){
				for(int j = 0; j < numRows; j++){
			    	if(s[index] != '\0' && j != 0 && j != numRows-1) {
			    		matrix[(numRows-j-1)*num*2*s_c + i*s_c] = s[index];
						index ++;
					}
				}	
			}
		}	
		
		string result = "";
		for(int j = 0; j < numRows; j++){
			for(int i = 0; i < num*2; i++){
				if(matrix[j*num*2*s_c + i*s_c] != ' ')
					result.append(1, matrix[j*num*2*s_c + i*s_c]);
			}
		}
		
		delete [] matrix;
		return result;
    }
};
```

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
  
  