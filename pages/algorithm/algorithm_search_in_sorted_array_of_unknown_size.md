---
title: "Search in a Sorted Array of Unknown Size"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_search_in_sorted_array_of_unknown_size.html
folder: algorithm
toc: false
---

## Description
Given an integer array sorted in ascending order, write a function to search target in nums.  If target exists, then return its index, otherwise return -1. However, the array size is unknown to you. You may only access the array using an `ArrayReader` interface, where `ArrayReader.get(k)` returns the element of the array at index k (0-indexed).
* If you access the array out of bounds, `ArrayReader.get` will return 2147483647, which is `Integer.MAX_VALUE`
* All the elements in the array are unique
* All integers in the array are in the range [-9999, 9999]

### Example
略

## Solution
用binary search做。第一步：找到右边界，或者说找到任何一个向右出了界的坐标。第二步：用典型的二分法查找target

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {
    public int search(ArrayReader reader, int target) {
        if (reader.get(0) == Integer.MAX_VALUE) {
            return -1;
        }
        
        int right = 1;
        while (reader.get(right) != Integer.MAX_VALUE) {
            right *= 2;
        }
        
        int left = 0;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (reader.get(mid) == Integer.MAX_VALUE || reader.get(mid) > target) {
                right = mid;
            } else if (reader.get(mid) < target) {
                left = mid;
            } else { // reader.get(mid) == target
                return mid;
            }
        }
        
        if (reader.get(left) == target) {
            return left;
        }
        if (reader.get(right) == target) {
            return right;
        }
        return -1; // 到最后也没找到
    }
}
```

## Reference
* [Search in a Sorted Array of Unknown Size [LeetCode]](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/description/)

{% include links.html %}
