---
title: "First and Last Position of Target in a Sorted Array"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_first_and_last_position_of_target_in_array.html
folder: algorithm
toc: false
---

## Description
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value. Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

### Example
```
* Input: nums = [5,7,7,8,8,10], target = 8
  * Output: [3,4]

## Solution
Binary Search, 九章style

注意，如果找不到target的第一个position，那么意思就是找不到target，那么就不用再找所谓的最后一个target了，直接返回 [-1, -1] 就行了

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[]{-1, -1};
        }
        
        int n = nums.length;
        int first = -1, last = -1;
        int left = 0, right = n - 1;
        
        // find first position
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) { // 注意这里和下面 find last 处的区别
                right = mid;
            } else { // nums[mid] < target
                left = mid;
            }
        }
        if (nums[left] == target) {
            first = left;
        } else if (nums[right] == target) {
            first = right;
        } else {
            return new int[]{-1, -1};
        }
        
        // find last position
        left = 0;
        right = n - 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid;
            } else { // nums[mid] <= target
                left = mid;
            }
        }
        if (nums[right] == target) {
            last = right;
        } else { // nums[left] == target
            last = left;
        }
        
        return new int[]{first, last};
    }
}
```

## Reference
* [Find First and Last Position of Element in Sorted Array [LeetCode]](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

{% include links.html %}
