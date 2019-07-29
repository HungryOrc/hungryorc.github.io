---
title: "Unique Paths"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_uniquePaths_acrossAMatrix.html
folder: algorithm
toc: false
---

## Description
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

Note: m and n will be at most 100.

### Example
略

## Solution 1: DP，速去 前1%

### Complexity
* Time: O(n * m)
* Space: O(n * m), dp matrix

### Java
```java
public class Solution {
  
  public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    
    for (int i = 0; i < m; i++) {
      dp[i][0] = 1;
    }
    for (int j = 0; j < n; j++) {
      dp[0][j] = 1;
    }
    
    for (int i = 1; i < m; i++) {
      for (int j = 1; j < n; j++) {
        dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
      }
    }
    return dp[m - 1][n - 1];
  }
}
```

## Reference
* [Unique Paths [LeetCode]](https://leetcode.com/problems/unique-paths/description/)

{% include links.html %}
