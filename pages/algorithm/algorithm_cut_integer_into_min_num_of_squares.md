---
title: "Cut Integer into Min Number of Squares"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cut_integer_into_min_num_of_squares.html
folder: algorithm
toc: false
---

## Description
比如
* 4 = 2^2 或 1^2 + 1^2 + 1^2 + 1^2，前者意思是切分为1块，后者为4块，所以答案是最小可能的切分是1块
* 10 = 3^2 + 1^2，答案是2 

### Example
如上

## Solution 1: 一维DP
见代码

### Complexity
* Time: O(n^2)，两层 for loop
* Space: O(n)，dp array

### Java
```java
public class Solution {
    public int minNumOfSquares(int num) {
        if (num <= 0) {
            return 0;
        }
        
        int[] dp = new int[num + 1]; // 从 0 到 num
        
        // base case 1
        for (int i = 1; i <= num; i++) {
            dp[i] = i; // at least we can cut an "i" into all 1's
        }
        
        // bas2 case 2
        int sqrt = 1;
        while (sqrt * sqrt <= num) {
            dp[sqrt * sqrt] = 1;
            sqrt++;
        }
        
        for (int i = 1; i <= num; i++) {
            for (int j = 1; j <= i - 1; j++) {
                dp[i] = Math.min(dp[i], dp[j] + dp[i - j]); // 关键在此
            }
        }
        return dp[num];
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
