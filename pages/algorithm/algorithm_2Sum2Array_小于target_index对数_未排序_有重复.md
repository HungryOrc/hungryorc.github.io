---
title: "2 Sum, 2 Arrays, Number of Pairs of Indexes，未排序，有重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum2Array_小于target_index对数_未排序_有重复.html
folder: algorithm
toc: false
---

## Description
2个数组，没排序，每个数组以内及两个数组之间都可能存在重复的元素。从2个数组里各取一个数组成一对。问一共有多少对的和 <= target。
一对的意思是一对indices，所以重复的值也是不同的对。但不能从一个数组里取两个数组成一对。

### Example
* Input: int[] nums1 = {5,9,2}, int[] nums2 = {-1,-3,4,-1}, target = 8
  * Output: 10

## Solution 1: 将两个数组都排序，然后2个数组各用一个指针，一个从后往前，一个从前往后，查看其和
两个数组排序以后，先看第一个数组，从最右边的即最大的开始，看第二个数组里有多少个和它的和 <= target。那么第二个数组就要从左往右即从小到大地看。

### Complexity
* Time: O(n1 * logn1 + n2 * logn2 + n1 + n2);
* Space: O(1)，双指针

### Java
```java
public class Solution {
    public int allPairs(int[] nums1, int[] nums2, int target) {
        // ignore sanity checks
        
        int n1 = nums1.length;
        int n2 = nums2.length;
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        
        int i1 = n1 - 1; // 从右往左，即从大到小
        int i2 = 0; // 从小到大
        int count = 0;
        
        for (; i1 >= 0; i1--) {
            int cur1 = nums1[i1];
            
            while (i2 < n2 && nums2[i2] + cur1 <= target) {
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
