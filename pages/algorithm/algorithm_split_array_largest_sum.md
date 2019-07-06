---
title: "Split Array Largest Sum"
tags: [algorithm, dynamic_programming, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_split_array_largest_sum.html
folder: algorithm
toc: false
---

## Description：Hard 题
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Note:
* If n is the length of array, assume the following constraints are satisfied:
  * 1 ≤ n ≤ 1000
  * 1 ≤ m ≤ min(50, n)

### Example
* Input: nums = [7,2,5,10,8], m = 2
  * Output: 18. There are four ways to split nums into two subarrays. The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

## Solution：DP
dp[i][j]：数组里 从index 0 到 index i（两端都inclusive）的这些数，正好分成j个groups，那么最大的group sum 的最小值是多少

### Complexity
* Time: O(m n^2)，n是数组里有多少个数，m是groups的个数
* Space: O(m n)

### Java
代码不很长，但思路比较巧妙，也很典型，值得反复品味
```java
class Solution {
    public int splitArray(int[] nums, int m) {
        // we had a guarantee that m is <= nums.length
        if (nums == null || nums.length == 0 || m <= 0) {
            return 0;
        }
        
        int n = nums.length;
        
        int[] prefixSums = new int[n];
        prefixSums[0] = nums[0];
        for (int i = 1; i < n; i++) {
            prefixSums[i] = nums[i] + prefixSums[i - 1];
        }
        
        int[][] dp = new int[n][m + 1];
        
        // base case：只有1个group的情况
        for (int i = 0; i < n; i++) {
            dp[i][1] = prefixSums[i];
        }
        // base case：只有1个item的话，group的个数>=2的话，最小的group的和一定都是0
        
        int result = Integer.MAX_VALUE;
        
        // 从2个group开始
        for (int groups = 2; groups <= m; groups++) {
            // i从左边数第2个数开始，当前的考察结束于i，即i是最右端
            for (int i = 1; i < n; i++) {
                dp[i][groups] = Integer.MAX_VALUE;
                
                // index 0 到 index k 为一部分，这个部分里可能有1个或多个组，
                // index k+1 到 index i 为另一部分，作为一个group
                for (int k = 0; k < i; k++) {
                    // 按照这种分割方法，最大的group的sum是多少
                    int maxGroupByThisDivision = 
                        Math.max(dp[k][groups - 1], prefixSums[i] - prefixSums[k]);
                    
                    dp[i][groups] = 
                        Math.min(dp[i][groups], maxGroupByThisDivision);
                }
            }
        }
        return dp[n - 1][m];
    }
}
```

## Reference
* [Split Array Largest Sum [LeetCode]](https://leetcode.com/problems/split-array-largest-sum/description/)

{% include links.html %}
