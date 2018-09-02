---
title: "Find a Local Peak in an Array with random ups and downs"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_local_peak_in_array.html
folder: algorithm
toc: false
---

## Description
一个整数数组 A，相邻两个元素之间不相等，但整个数组里可能存在重复元素。整个数组的元素从左到右大小变化不定，
但是第一个和最后一个元素满足：`A[0] < A[1] && A[A.length - 2] > A[A.length - 1]`。

要求找到任何一个局部最大值，需要满足：`A[i] > A[i - 1] && A[i] > A[i + 1]`。返回这个 local peak 的index。

### 联系类似题目
如果数组的第一个和最后一个元素可以是 local peak，那么做法也是基本一样的。

### Example
* Input: [1, 2, 3, 1, 5, 6, -4, 0]
  * Output: 2 (index of 3) or 5 (index of 6)

## Solution 1
`A[0] < A[1] && A[A.length - 2] > A[A.length - 1]` 这个条件是非常关键的。它不仅显示了数组的第一个和最后一个元素都不可能是 local peak，
而且更重要的是揭示了如下规律：
* 如果某个元素a比它左边相邻的元素小，那么a与数组的开头之间（不包括a本身），一定存在至少一个peak
* 如果某个元素a比它左边相邻的元素大，那么a与数组的结尾之间（包括a本身），一定存在至少一个peak

用 iteration 的方式来做。

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {

    public int findPeak(int[] A) {
        if (A == null || A.length < 3) {
            return null;
        }
    
        // 题意说了，左右边界处不会是peak
        int left = 1, right = A.length-2; 
        
        while(start + 1 < end) {
            int mid = left + (right - left) / 2;
            
            // 题意说了，相邻元素之间不存在相等的情况
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

## Solution 2
思路和前面的Solution是一样的。只是用 recursion 的方式来做。

Binary Search 用 resursion 做是有点别扭，权当练习。

recursion做法竟然比 iteration做法快，beat LeetCode 99%，我感觉如果不用 call stack 应该会更快，为什么会反过来 ？？？

### Complexity
* Time: O(logn)
* Space: O(logn)，这是因为call stack 共有 logn 层，每层里面都是 O(1) 的空间消耗

### Java
```java
class Solution {

    public int findPeak(int[] nums) {
        if (nums == null || nums.length < 3) {
            return null;
        }
        
        // 从第二个元素开始，到倒数第二个元素结束搜索。因为题目说了第一个和最后一个不是
        return findAnyPeak(nums, 1, nums.length - 2);
    }
    
    private int findAnyPeak(int[] nums, int left, int right) {
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[mid - 1]) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        
        return (nums[left] < nums[right]) ? right : left;
    }
}
```

## Reference
* [Find Peak Element [LeetCode]](https://leetcode.com/problems/find-peak-element/description/)

{% include links.html %}
