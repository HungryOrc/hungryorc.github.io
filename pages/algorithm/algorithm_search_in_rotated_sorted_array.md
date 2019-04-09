---
title: "Search in Rotated Sorted Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_search_in_rotated_sorted_array.html
folder: algorithm
toc: false
---

## Description
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand. 
(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.

Your algorithm's **runtime complexity must be in the order of O(log n)**.

### Example
* Input: nums = [4,5,6,7,0,1,2], target = 0
  * Output: 4

## Solution 1: 只用一次二分法！巧妙。速度前1%

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }

        int start = 0;
        int end = nums.length - 1;
        int mid;
        
        while (start + 1 < end) {
            mid = start + (end - start) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // 接下来就不直接比较mid和target的值了！
            // 而是先要比较start和mid的值，以先确认start与mid所在的分段的情况！
            // 然后才是比较start，mid，end与target的值！
            
            // 第一种情况，start和mid在同一段上，这里又包含2种子情况：
            // 它们都在左半分段上，或者它们都在右半分段上
            // 但这2种子情况都可以用下述的方式统一处理：
            if (nums[start] < nums[mid]) {
                // 如果target处于start与mid之间
                if (nums[start] <= target && target <= nums[mid]) {
                    end = mid;
                } 
                // 否则target只可能在mid的右边（同时在end的左边）了，因为target不可能在start的左边
                else {
                    start = mid;
                }
            
            // 第二种情况，start和mid不在同一段上；确切地说：
            // start在左半分段上，mid, end 在右半分段上
            } else {
                // 如果target处于mid与end之间
                if (nums[mid] <= target && target <= nums[end]) {
                    start = mid;
                } 
                // 否则target只可能在mid的左边（同时在start的右边）了，因为target不可能在end的右边
                else { 
                    end = mid;
                }
            }
        }
        
        if (nums[start] == target) {
            return start;
        }
        if (nums[end] == target) {
            return end;
        }
        return -1;
    }
}
```

## Solution 2: 老老实实用两次二分法，速度比前面慢点，但容易理解

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
public class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        
        // 如果数组真的被翻转过
        if (nums[0] > nums[nums.length - 1])
        {
            // find the start of the right half array
            int rightHalfEndValue = nums[nums.length - 1];
            while (start < end) {
                int mid = start + (end - start) / 2;
                if (nums[mid] > rightHalfEndValue) {
                    start = mid + 1;
                } else { // if (mid <= rightHalfEndValue)
                    end = mid;
                }
            }
            // now start == end
            int rightHalfBeginValue = nums[start];

            // locate the half in which the target is in
            if (target < rightHalfEndValue) {
                // start 不变，此时start就等于right half的起始index
                end = nums.length - 1;
            } else if (target > rightHalfEndValue) {
                start = 0;
                end -= 1;
            } else { // ==
                return nums[nums.length - 1];
            }
        }
    
        // find the index of the target
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < target) {
                start = mid + 1;
            } else if (nums[mid] > target) {
                end = mid - 1;
            } else { // ==
                return mid;
            }
        }
        // now start == end
        if (nums[start] == target) {
            return start;
        } else {
            return -1;
        }
    }
}
```

## Reference
* [Search in Rotated Sorted Array [LeetCode]](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)

{% include links.html %}
