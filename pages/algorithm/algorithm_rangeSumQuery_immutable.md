---
title: "Range Sum Query - Immutable"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rangeSumQuery_immutable.html
folder: algorithm
toc: false
---

## Description: Easy
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

```java
// Your NumArray object will be instantiated and called as such:
NumArray obj = new NumArray(nums);
int param_1 = obj.sumRange(i,j);
```

Note:
* You may assume that the array does not change.
* There are many calls to sumRange function.

### Example
* Input: nums = [-2, 0, 3, -5, 2, -1]
  * Output: 
    ```
    sumRange(0, 2) -> 1
    sumRange(2, 5) -> -1
    sumRange(0, 5) -> -3
    ```

## Solution 1: 用 prefix sum 做，速度 前20%

### Complexity
* Time: O(1)，每次求range sum 都是
* Space: O(n)

### Java
```java
class NumArray {
    int[] prefixSums;
    
    public NumArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            this.prefixSums = null;
            return;
        }
        
        int n = nums.length;
        this.prefixSums = new int[n];
        this.prefixSums[0] = nums[0];
        
        for (int i = 1; i < n; i++) {
            prefixSums[i] = prefixSums[i - 1] + nums[i];
        }
    }
    
    public int sumRange(int i, int j) {
        if (this.prefixSums == null) return Integer.MIN_VALUE;
        
        if (i == 0) return prefixSums[j]; // 注意别忘了这种情况
        
        return prefixSums[j] - prefixSums[i - 1];
    }
}
```

## Reference
* [Range Sum Query - Immutable [LeetCode]](https://leetcode.com/problems/range-sum-query-immutable/description/)

{% include links.html %}
