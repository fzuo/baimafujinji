---
layout: post
title: 你是否真正掌握了Binary Search
category: 算法与结构
tags: 二分搜索
keywords: 算法,二分搜索
---


二分查找（Binary Search）是数据结构与算法课程中应该讲到的一个非常重要的内容。通常，二分查找算法可以借由递归的结构来实现，也可以通过迭代来实现。但是在实际应用中二分查找可能变化出许多更为复杂的形式，面对这些变种题目时能否得心应手地来处理，便考察了你是否真正掌握了二分查找的精髓。

关于二分查找（Binary Search）的基本内容，我们不打算在此赘述，如果你对这个问题本身不甚了解，也可以参考《算法之美——隐匿在数据结构背后的原理（C++版）》一书163页对应之内容。而本文主要讨论几个常见的二分查找变种题目。

言归正传，首先来看一道Leetcode题目（该题目的难道等级为Medium）:

显然，这道题目的意思是在说给定一个有序（非降序）数组 nums 和一个 target，如果target在数组中出现，返回位置，若不存在，返回它应该插入的位置。注意不能直接采用线性方法，本文中所有的题目都要使用Binary Search的方法来解决。

这里给出一个非递归的版本：

```c
int searchInsert(int* nums, int numsSize, int target) {
    int low=0, high=numsSize-1;
    if(target > *(nums+high))  return high+1;  
    if(target < *(nums+ low))  return low; 
    
    while(low<=high)
    {
        int mid=(low+high)/2;
        
        if(nums[mid]<target)
            low =mid+1;    
        if(nums[mid]>target)    
            high=mid-1;
        if(nums[mid]==target)    
            return mid; 
    }
    return low;
}
```

下面我们把上述问题扩展，来看几个变种题目。
1、给定一个有序（非降序）数组 nums，可含有重复元素，求最小的下标 i 使得nums[i]等于target，不存在则返回-1。

就问题本身而言，这显然是要求target在数组中第一次出现的位置。注意，直接使用原始的二分查找并不可行。

```c
#include <stdio.h>

int searchFirstPos(int* nums, int numsSize, int target)
{
    if(numsSize <= 0) return -1;
    int low = 0, high = numsSize-1;
    
    while(low < high)
    {
        int mid = low +((high-low)>>1);
        
        if( *(nums+mid) < target)
            low = mid+1;
        else // nums[mid] >= target
            high = mid;
    }

    if(*(nums+low) != target)
        return -1;
    else
        return low;
}

int main(int argc, const char * argv[]) {
    
    int array[9] = {0, 1, 2, 3, 3, 3, 4, 5, 6};
    printf("%d\n", searchFirstPos(array, 9, 3));
    return 0;
}
```

2、给定一个有序（非降序）数组nums，可含有重复元素，求最大的下标 i 使得nums[i]等于target，不存在则返回-1。

此题是求target在数组中最后一次出现的位置。处理思路与上一题基本一样，但是有个tricky是地方需要注意。主要是计算中间位置时不能用low+((high-low)>>1)，因为当low+1等于high且nums[low] <= target时，会导致死循环；所以这里要使用low+((high-low+1)>>1)，这样能够保证循环会正常结束。

```c
#include <stdio.h>

int searchLastPos(int* nums, int numsSize, int target)
{
    if(numsSize <= 0) return -1;
    int low = 0, high = numsSize-1;
    
    while(low < high)
    {
        int mid = low+((high-low+1)>>1);
        
        if(nums[mid] > target)
            high = mid-1;
        else // nums[mid] <= target
            low = mid;
    }

    if(*(nums+high) != target)
        return -1;
    else
        return high;
}


int main(int argc, const char * argv[]) {
    
    int array[9] = {0, 1, 2, 3, 3, 3, 4, 5, 6};
    printf("Last Position is %d\n", searchLastPos(array, 9, 3));
    return 0;
}
```

3、给定一个有序（非降序）数组nums，可含有重复元素，求target在数组中出现的次数。

当然我们不能去一个一个数target出现了多少次，因为这样的复杂度是O(n)。如果要采样二分查找的方法，可以求出第一次出现位置和最后一次出现位置，然后再做差即可。由于searchFirstPos()和前面都已实现，这里不多解释。



<span style="color:blue">**（本文完）**</span>
