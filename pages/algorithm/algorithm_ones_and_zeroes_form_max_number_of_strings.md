---
title: "Use Ones and Zeroes to Max Number of Form Strings"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_ones_and_zeroes_form_max_number_of_strings.html
folder: algorithm
toc: false
---

## Description
In the computer world, use restricted resource you have to generate maximum benefit is what we always want to pursue.

For now, suppose you are a dominator of m 0s and n 1s respectively. On the other hand, there is an array with strings consisting of only 0s and 1s.

Now your task is to find the maximum number of strings that you can form with given m 0s and n 1s. Each 0 and 1 can be **used at most once**.
* The given numbers of 0s and 1s will both not exceed 100
* The size of given string array won't exceed 600.

### Example
* Input: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
  * Output: 4
  * At most 4 strings can be formed by the using of 5 0s and 3 1s, which are “10,”0001”,”1”,”0”
* Input: Array = {"10", "0", "1"}, m = 1, n = 1
  * Output: 2
  * You could form "10", but then you'd have nothing left, and it's only one number. Better form "0" and "1", that's two numbers.

## Solution 1: 三维DP。用了“Offset One”的方法，即把“零”而非第一个元素作为某一个或几个维度的开始
这一题只要用 m个0 和 n个1 凑String数组里的strings，所以每个string里的0和1是怎么排列的根本不care，只care每个string里有几个0，几个1

这题明显用DP做。难点在于这是一个三维DP，一开始不是很好想到这种三维结构
* dp[len + 1][m + 1][n + 1]，其中 dp[i][j][k] 表示 用 j个0 和 k个1，最多能组成String数组里 前i个元素 中的几个。注意这里是 前i个元素，不是元素index从0到i。

注意
* m 和 n 都是可以等于 0 的

### Complexity
* Time: O(len * m * n), len是String数组的长度
* Space: O(len * m * n)

### Java
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0 || m < 0 || n < 0) {
            return 0;
        }
        
        int len = strs.length;
        int[][] counts = new int[len][2];
        
        // fill in the counts matrix
        for (int i = 0; i < len; i++) {
            for (char c : strs[i].toCharArray()) {
                if (c == '0') {
                    counts[i][0]++;
                } else { // c == '1'
                    counts[i][1]++;
                }
            }
        }
        
        int[][][] dp = new int[len + 1][m + 1][n + 1];
        
        for (int i = 1; i <= len; i++) {
            int zeroNum = counts[i - 1][0];
            int oneNum = counts[i - 1][1];
            
            for (int j = 0; j <= m; j++) { // number of zeroes
                for (int k = 0; k <= n; k++) { // number of ones
                    
                    dp[i][j][k] = dp[i - 1][j][k];
                    
                    if (j >= zeroNum && k >= oneNum) {
                        dp[i][j][k] = Math.max(dp[i][j][k], dp[i - 1][j - zeroNum][k - oneNum] + 1);
                    }
                }
            }
        }
        return dp[len][m][n];
    }
}
```

## Reference
* [Ones and Zeroes [LeetCode]](https://leetcode.com/problems/ones-and-zeroes/description/)

{% include links.html %}
