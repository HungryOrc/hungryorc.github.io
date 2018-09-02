---
title: "Find a Global Peak in an A-Shape Array with Duplicate Elements"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_global_peak_in_A-shape_array_with_duplicates.html
folder: algorithm
toc: false
---

## Description
一个整数数组 nums，有重复元素，整个数组的元素值先升后降，数组长度 >= 3。

要求找到全局最大值，返回其 index。

### 相似题目
数组先升后降，但是没有重复元素，找 global peak 那一题

### Example
* Input: [1, 2, 2, 2, 6, 4, 0, 0]
  * Output: 4 (index of 6)

## Solution
还是用二分法。但是如果一个元素的左边及右边相邻的元素都小于等于它，并不能判定这个元素就是 global peak。因为它们可能只是局部平台而已。

比如 `… 3, 2, 2, 1…` 那么第二个2可能被误判为全局峰值，因为它左边的2小于等于它，它右边的1也小于等于它.


二分法：
* 如果某个元素a比它左边相邻的元素小，那么a与数组的开头之间（不包括a本身），必然含有 global peak
* 如果某个元素a比它左边相邻的元素大，那么a与数组的结尾之间（包括a本身），必然含有 global peak

用 iteration 的方式来做。

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {

    public int findGlobalPeak(int[] nums) {
        if (nums == null || nums.length < 3) {
            return Integer.MIN_VALUE;
        }
    
        // 题意说了，数组最少长度为3，左右边界处不会是peak
        int left = 1, right = A.length-2; 
        
        while(start + 1 < end) {
            int mid = left + (right - left) / 2;
            
            // 所有元素互不相等
            if(A[mid] < A[mid - 1]) {
                right = mid - 1;
            } else { // A[mid] > A[mid - 1]
                left = mid;
            }
        }
        
        return (A[left] < A[right]) ? right : left;
    }
}
```

## Reference

{% include links.html %}
