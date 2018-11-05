---
title: "Search Insert Position"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_search_insert_position.html
folder: algorithm
toc: false
---

## Description
Given a sorted array and a target value, return the index if the target is found. 
If not found, return the index where we should insert it at, this should be the index of the existing smallest element
among the elements that are larger than the target.

You may assume NO duplicates in the array.

### Example
* Input: 1,3,5,6], 5
  * Output: 2
* Input: 1,3,5,6], 7
  * Output: 4
* Input: 1,3,5,6], -2
  * Output: 0

## Solution
binary search

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
public class Solution {
    public int searchInsert(int[] A, int target) {
        if (A == null) {
            return -1;
        }
        // 如果当前数组为空，则插入到第一位
        if (A.length == 0) {
            return 0;
        }
        
        // 这里用的是binary search的经典模板
        int left = 0, right = A.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (A[mid] > target) {
                right = mid - 1;
            } else if (A[mid] < target) {
                left = mid + 1;
            } else { // A[mid] == target
                return mid; // 找到了target，就直接return
            }
        }
        
        // 到了这里意味着没有找到target
        // 此时left = right + 1，即left所指示的是
        // 大于target的最靠左的数
        return left;
    }
}
```

## Reference
* [Search Insert Position [LintCode]](https://www.lintcode.com/problem/search-insert-position/description)

{% include links.html %}
