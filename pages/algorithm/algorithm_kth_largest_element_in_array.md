---
title: "Find the Kth Largest Element in an Unsorted Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_kth_largest_element_in_array.html
folder: algorithm
toc: false
---

## Description
Find the kth largest element in an unsorted array. There might be duplicate elements in the array. 
You may assume that k is always valid, namely 1 <= k <= array's length.

Note that **it is the kth largest element in the sorted order, not the kth distinct element**.

这一题是找第k大的那一个数，而非k个最大的数

### Example
* Input: [-3,0,3,1,2,1,1,4,5,15,6] and k = 4
  * Output: 1

## Solution 2: Quick Select，速度较慢，后20%
算法笔记里专门有一篇讲 Quick Select，可以找那个看看。

### Complexity
* Time: 最优以及平均 O(n)，最差 O(n^2)
* Space: O()

### Java
```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return Integer.MIN_VALUE;
        }
        
        // 找第k个最大，就是找第length-k+1个最小
        // quick select原本是用来找第k个最小，所以在这里转化一下
        return quickSelect(nums, 0, nums.length - 1, nums.length - k + 1);
    }
    
    // quick select 这个算法的定义就是 “找第 k 小的数”，k 也就是下面的最后一个参数
    private int quickSelect(int[] nums, int start, int end, int k) {
        int chosenIndex = partition(nums, start, end);
        
        if (chosenIndex < k - 1) { // 第 k 小的数的 index 是 k - 1
            return quickSelect(nums, chosenIndex + 1, end, k);
        } else if (chosenIndex > k - 1) {
            return quickSelect(nums, start, chosenIndex - 1, k);
        } else { // chosenIndex == k - 1
            return nums[chosenIndex];
        }
    }
    
    // 和一般的 quick sort 的 partition函数 一样
    // 这个函数一方面会把小于pivot的数都放到左半边，另一方面会把pivot的index返回来
    // 在下面的 partition 的实现里，pivot的选取是选数组里的最右边的数。可以用别的选法，比如随机
    private int partition(int[] nums, int start, int end) {
        int left = start;
        int right = end - 1; // 别忘了 -1，因为 pivot取为最后一个数了，所以right只能取为倒数第二个
        int pivot  = nums[end];
        
        while (left <= right) {
            if (nums[left] < pivot) {
                left ++;
            } else if (nums[right] > pivot) {
                right --;
            } else {
                swap(nums, left++, right--);
            }
        }
        
        // 最后left要么是紧贴着right并且在right的右边一位，要么是left和right重合
        swap(nums, left, end);
        return left;
    }
    
    private void swap(int[] nums, int i1, int i2) {
        int tmp = nums[i1];
        nums[i1] = nums[i2];
        nums[i2] = tmp;
    }
}
```

## Reference
* [Kth Largest Element in an Array [LeetCode]](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

{% include links.html %}
