---
title: "Find a Local Peak in an Array with random ups and downs"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_peak_in_array_local.html
folder: algorithm
toc: false
---

## Description
一个整数数组 A，相邻两个元素之间不相等，但整个数组里可能存在重复元素。整个数组的元素从左到右大小变化不定，
但是第一个和最后一个元素满足：`A[0] < A[1] && A[A.length - 2] > A[A.length - 1]`。

要求找到任何一个局部最大值，需要满足：`A[i] > A[i - 1] && A[i] > A[i + 1]`。返回这个 local peak 的index。

## Example
* Input: [1, 2, 3, 1, 5, 6, -4, 0]
  * Output: 2 (index of 3) or 5 (index of 6)

## Solution
`A[0] < A[1] && A[A.length - 2] > A[A.length - 1]` 这个条件是非常关键的。它不仅显示了数组的第一个和最后一个元素都不可能是 local peak，
而且更重要的是揭示了如下规律：
* 如果某个元素a比它左边相邻的元素小，那么a与数组的开头之间（不包括a本身），一定存在至少一个peak
* 如果某个元素a比它左边相邻的元素大，那么a与数组的结尾之间（包括a本身），一定存在至少一个peak！

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {

    public int findPeak(int[] A) {
        if (A == null || A.length < 3) {
            return null;
        }
    
        // 题意说了，左右边界处不会是peak
        int start = 1, end = A.length-2; 
        
        while(start + 1 < end) {
            int mid = start + (end - start) / 2;
            
            // 题意说了，相邻元素之间不存在相等的情况
            if(A[mid] < A[mid - 1]) {
                end = mid;
            } else { // A[mid] > A[mid - 1]
                start = mid;
            }
        }
        
        return (A[start] < A[end]) ? end : start;
    }
}
```

## Reference
* [Find Peak Element [LeetCode]](https://leetcode.com/problems/find-peak-element/description/)

{% include links.html %}
