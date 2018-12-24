---
title: "Paint House I"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_paint_house.html
folder: algorithm
toc: false
---

## Description
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note: All costs are positive integers.

### Example
* Input: [[17,2,17],[16,16,5],[14,3,19]]
  * Output: 10. Paint house 0 into blue, paint house 1 into green, paint house 2 into blue. Minimum cost: 2 + 5 + 3 = 10.

## Solution: DP，前1%速度
挺简明的逻辑。把3种颜色分开处理

### Complexity
* Time: O(n * m * (m - 1)) = O(n * m^2)，其中 n 是房子的个数，m 是颜色的个数
* Space: O(n * m)

### Java
```java
class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) {
            return 0;
        }
        
        int n = costs.length; // number of houses
        int m = 3; // number of colors
        
        int[][] dp = new int[n][m];
        
        // base case
        dp[0][0] = costs[0][0];
        dp[0][1] = costs[0][1];
        dp[0][2] = costs[0][2];
        
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.min(dp[i - 1][1], dp[i - 1][2]) + costs[i][0];
            dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][2]) + costs[i][1];
            dp[i][2] = Math.min(dp[i - 1][1], dp[i - 1][0]) + costs[i][2];
        }
        
        return Math.min(dp[n - 1][0], Math.min(dp[n - 1][1], dp[n - 1][2]));
    }
}
```

## Reference
* [Paint House [LeetCode]](https://leetcode.com/problems/paint-house/description/)

{% include links.html %}
