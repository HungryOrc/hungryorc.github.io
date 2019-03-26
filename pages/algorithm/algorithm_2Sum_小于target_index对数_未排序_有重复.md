---
title: "2 Sum, 小于target，Number of Pairs of Indexes，未排序，有重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_小于target_index对数_未排序_有重复.html
folder: algorithm
toc: false
---

## Description
1个数组，没排序，可能存在重复的元素。问一共有多少对元素的和 <= target。
一对的意思是一对indices，所以重复的值也是不同的对。

### Example
略

## Solution: 将数组排序，然后2个指针，一个从后往前，一个从前往后，查看其和
从最右边的即最大的开始，左边的指针从0开始往右移，看最多移动到什么位置的时候，不再满足 两个数相加 <= target

### Complexity
* Time: O(n logn + n);
* Space: O(1)，双指针

### Java
```java
public class Solution {
    public int allPairs(int[] nums, int target) {
        // ignore sanity checks
        
        int n = nums.length;
        Arrays.sort(nums);
        
        int i1 = n - 1; // 从右往左，即从大到小
        int i2 = 0; // 从小到大
        int count = 0;
        
        for (; i1 >= 0; i1--) {
            int cur = nums[i1];
            
            while (i2 < n && nums[i2] + cur <= target) {
                i2++;
            }
            i2--;
            count += (i2 + 1);
        }
        return count;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
