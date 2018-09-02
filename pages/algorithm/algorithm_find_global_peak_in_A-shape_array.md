---
title: "Find a Global Peak in an A-Shape Array"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_global_peak_in_A-shape_array.html
folder: algorithm
toc: false
---

## Description
一个整数数组 nums，整个数组里没有重复元素，整个数组的元素值先升后降，数组长度 >= 3。

要求找到任何一个全局最大值，返回其 index。

### 联系类似题目
这一题的做法和那种”整个数组的元素大小变化不定，但只要找到任意一个 local peak“的题目的做法是一样的。

### Example
* Input: [1, 2, 3, 1, 0]
  * Output: 2 (index of 3)

## Solution
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
            return null;
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
