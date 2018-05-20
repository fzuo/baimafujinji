---
layout: post
title: 数组排序并返回元素最初的位置
category: 算法与结构
tags: 数组排序
keywords: 数组排序,结构体
---


对数组中的元素进行排序是非常基础的问题，从现有的众多排序算法中任选一种即可实现。但是如果要求排序后同时返回新的（有序）数组中每个元素在旧的（未排序）数组中之原始位置，问题可能就变得有些棘手了。

例如假设有一个数组如下：

`[2, 30, 1, 5, 12, 8, 6]`

排序（从小到大）后可得新的数组如下：

`[1, 2, 5, 6, 8, 12, 30]`

那么对于新数组中值等于1的元素而言，其在原数组中的位置就应该是2（注意数组下标从0开始计数）。


当然，最straightforward的想法就是回到原数组中去search，从而获得元素在其中的位置。但这样做的代价显然有点高，比较对每个元素都做一次搜索还是相当浪费的。从执行时间复杂度来看，有没有更好的办法呢？所以更常被使用的方法是使用一个结构体数组，例如下面这种形式：


```cpp
struct value_index_pair
{
    double value;
    int index;
};
```

也就是说，对于原数组中的每个元素，我们在一个结构体中既存放它的值（value），也存放它在数组中的位置（index）。接下来的问题就相对简单了。当我们执行排序算法的时候，如果需要将两个元素交换位置，那么我们不仅会交互这两个元素的value，同时也会交换它们的index。这样就方便很多了！


来看下面这段示例代码。因为排序算法本身并不是我们所关注的焦点，所以我们之间调用C语言的库函数qsort()来执行排序过程。

```cpp
#include <stdio.h>
#include <stdlib.h>

#define SIZE 7

int comp(const void *a, const void *b)
{
    return (*(struct value_index_pair *)a).value - (*(struct value_index_pair *)b).value;
}

int main(int argc, const char * argv[]) {
    
    struct value_index_pair * sequence = malloc(SIZE * sizeof(struct value_index_pair));
    
    struct value_index_pair * ps;
    ps = &sequence[0];
    
    double array[7] = {2.0, 30.0, 1.0, 5.0, 12.0, 8.0, 6.0};
    for(int i = 0; i < SIZE; i++)
    {
        ps[i].index = i;
        ps[i].value = array[i];
    }
    
    qsort(sequence, SIZE, sizeof(struct value_index_pair), comp);
    
    //输出排序后的结果
    for(int i = 0; i < SIZE; i++)
    {
        printf("[%d] value: %.1f index: %d\n", i, ps[i].value, ps[i].index);
    }
    
    free(sequence);
    sequence = NULL;
    
    return 0;
}
```

执行上述程序，可得输出结果如下：

```
[0] value: 1.0 index: 2
[1] value: 2.0 index: 0
[2] value: 5.0 index: 3
[3] value: 6.0 index: 6
[4] value: 8.0 index: 5
[5] value: 12.0 index: 4
[6] value: 30.0 index: 1
```

可见我们的程序执行正确。

<span style="color:blue">**（本文完）**</span>
