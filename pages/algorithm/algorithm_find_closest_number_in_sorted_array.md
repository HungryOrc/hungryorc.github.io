---
title: "Find the Closest Number in a Sorted Array"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_closest_number_in_sorted_array.html
folder: algorithm
toc: false
---

## Description
Given a target number and an integer array A sorted in ascending order, find the index i in A such that A[i] is closest to the given target.
Return -1 if there is no element in the array.
* Requires O(logn) time complexity
* If 2 elements are closest to the target and they have the same distance to the target, then return either one's index
* There can be duplicate elements in the array, and we can return any of their indices if they are the closest ones

### Example
* Input: Given [1, 4, 6], target = 2
  * Output: 0
* Input: Given [1, 4, 6], target = 5
  * Output: 1 or 2

## Solution
用九章的“双子”二分法模板，可以想到，最接近的数一定是最后的这两个数之一

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
public class Solution {
    public int findClosestNum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        // 九章双子，哦也
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] > target) {
                end = mid;
            } else if (nums[mid] < target) {
                start = mid;
            } else {
                return mid; // 如果正好等于target，那就不可能比这个更接近了
            }
        }
        
        if (Math.abs(nums[start] - target) >= Math.abs(nums[end] - target)) {
            return end;
        }
        return start;
    }
}
```

## Reference
* [Closest Number in Sorted Array [LintCode]](https://www.lintcode.com/problem/closest-number-in-sorted-array/)

{% include links.html %}
