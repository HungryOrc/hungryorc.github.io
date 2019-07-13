---
title: "Maximum Product Subarray"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_maxProduct_subarray.html
folder: algorithm
toc: false
---

## Description
Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

注意：
1. 这题可以有负整数，还可以有 0
2. 这题不会有小数，所以一个一个数往右累乘下去的时候，乘积的绝对值，只要不乘到0，那么就是单调递增的（乘到1不变）

### Example
* Input: [2,3,-2,4]
  * Output: 6. [2,3] has the largest product 6.
* Input: [-2,0,-1]
  * Output: 0. The result cannot be 2, because [-2,-1] is not a subarray.
* Input: [-2,10,12,-1]
  * Output: 120.

## Solution 1: DP，速度 前1%
要特别注意的是：如果 nums[i] 是一个负数，那么：
* 结尾在 nums[i-1] 处的所有可能的连乘积里面的 最大值，乘以 nums[i]，就会变成 结尾在 nums[i] 处的所有的连乘积里面的 最小值 
* 同理，结尾在 nums[i-1] 处的最小连乘积，乘以 nums[i]，就会变成 结尾在 nums[i] 处的最大连乘积

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int maxProduct(int[] nums) {
    	if (nums == null || nums.length == 0) {
		    return Integer.MIN_VALUE;
        }

        int max = nums[0]; // 全局最大
        
        // prevMin 是 连续任意个的 结尾在i-1处（含有i-1处的数）的数的连乘积，最小是多少
        // prevMax 是 连续任意个的 结尾在i-1处（含有i-1处的数）的数的连乘积，最大是多少
        int prevMin = nums[0], prevMax = nums[0];

        for (int i = 1; i < nums.length; i++) {
            int cur = nums[i];
            int curMax = cur, curMin = cur;

            if (cur < 0) {
                curMax = Math.max(prevMin * cur, cur);
                curMin = Math.min(prevMax * cur, cur);
            } else {
                curMax = Math.max(prevMax * cur, cur);
                curMin = Math.min(prevMin * cur, cur);
            }
            
            prevMax = curMax;
            prevMin = curMin;
            
            max = Math.max(max, prevMax);
        }
        return max;
        
    }
}
```

## Reference
* [Maximum Product Subarray [LeetCode]](https://leetcode.com/problems/maximum-product-subarray/description/)

{% include links.html %}
