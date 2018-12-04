---
title: "K Sum, Number of Solutions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_k_sum_number_of_solutions.html
folder: algorithm
toc: false
---

## Description
Given n **distinct** positive integers, integer k (k <= n) and a number target.

Find k numbers where sum is target. Calculate how many solutions there are?

注意
* 数组里没有重复的数字

### Example
* Input: [1,2,3,4], k = 2, target = 5.
  * Output: 2. Since there are 2 solutions: [1,4] and [2,3]

## Solution: DP, with a 3D array，速度 前15%

问题：下面这种DP做法，就算数组里有重复数字，也一样可以适用么 ？？？

三维DP。类似于 **背包问题，限制物品个数，限制总重量，求有多少种装法正好等于某个总重量** 的那类题目。
* 设给的数组长度 = n, DP矩阵为：`int dp[n + 1][k + 1][target + 1]`
* `dp[i][j][sum]`: 从数组里 index从0到i 的数里（这种做法下数组不用排序），正好使用了j个数，总和正好为sum，满足这三个条件的解法一共有多少种。
* Base Case
  * 先考察第二维为0，即取0个数的情况
    * `dp[i][0][0] = 1`, 0 <= i < n
  * 再考虑第一维为0，即只能最多取第一个数的情况，取或者不取。不取的情况上面其实已经有了
    * `dp[0][1][nums[0]] = 1`, if (nums[0] <= target)
* Induction Rules
  * 情况一：如果 array[i] <= sum
    * `dp[i][j][sum] = dp[i - 1][j][sum] + dp[i - 1][j - 1][sum - array[i - 1]]`
    * 等号右边加号左边是 没有使用 index=i 的这个数，加号右边是 使用了这个数。
  * 情况二：如果 array[i] > sum，
    * `dp[i][j][sum] = dp[i - 1][j][sum]`
    * 可见这个式子和上面那个式子的一部分是一样的，所以这两个式子可以合并

### Complexity
* Time: O(n * k * target)
* Space: O(n * k * target), the dp 3D array

### Java
```java
public class Solution {
    public int kSum(int[] nums, int k, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int n = nums.length;
        int[][][] dp = new int[n][k + 1][target + 1];
        
        // base cases
        // 先考察第二维为0，即取0个数的情况
        for (int i = 0; i < n; i++) {
            dp[i][0][0] = 1;
        }
        // 再考虑第一维为0，即只能最多取第一个数的情况，取或者不取
        // 不取的情况上面其实已经有了
        if (nums[0] <= target) {
            dp[0][1][nums[0]] = 1;
        }
        
        for (int i = 1; i < n; i++) { // i从1开始
            int cur = nums[i];
            
            for (int j = 1; j <= k; j++) { // j从1开始
                for (int s = 0; s <= target; s++) {
                    
                    dp[i][j][s] = dp[i - 1][j][s];
                    
                    if (cur <= s) {
                        dp[i][j][s] += dp[i - 1][j - 1][s - cur];
                    }
                    
                }
            }
        }
        return dp[n - 1][k][target];
    }
}
```

## Reference
* [K Sum [LintCode]](https://www.lintcode.com/problem/k-sum/description)

{% include links.html %}
