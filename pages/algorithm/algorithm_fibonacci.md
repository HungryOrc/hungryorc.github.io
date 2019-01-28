---
title: "Fibonacci Number"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_fibonacci.html
folder: algorithm
toc: false
---

## Description
The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is:
```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1
```
Given N, calculate F(N)

Note:
* F(0) = 0, F(1) = 1, F(2) = 1
* 0 <= N <= 30

### Example
略

## Solution 1: 记忆化。Dynamic Programming

### Complexity
* Time: O(n)
* Space: O(n)，dp数组

### Java
代码虽然看起来不算很短，但是思路其实很简单
```java
class Solution {
    public int fib(int n) {
        if (n < 0) {
            return Integer.MIN_VALUE;
        } else if (n == 0) {
            return 0;
        } else if (n <= 2) {
            return 1;
        }
        
        int[] dp = new int[n + 1];
        
        // 初始化
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            dp[i] = -1;
        }
        
        return calcFib(n, dp);
    }
    
    private int calcFib(int n, int[] dp) {
        if (dp[n] != -1) {
            return dp[n];
        }
        
        dp[n] = calcFib(n - 1, dp) + calcFib(n - 2, dp);
        return dp[n];
    }
}
```

## Reference
* [Fibonacci Number [LeetCode]](https://leetcode.com/problems/fibonacci-number/description/)

{% include links.html %}
