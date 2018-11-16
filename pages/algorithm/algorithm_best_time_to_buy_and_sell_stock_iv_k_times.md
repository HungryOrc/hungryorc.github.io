---
title: "Best Time to Buy and Sell Stock: K Transactions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_best_time_to_buy_and_sell_stock_iv_k_times.html
folder: algorithm
toc: false
---

## Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete **at most k transactions**.
一个`transaction`的意思是一次买加上一次卖。

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Example
* Input: [2,4,1], k = 2
  * Output: 2
  * Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2
* Input: [3,2,6,5,0,3], k = 2
  * Output: 7
  * Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0]
  * Output: 0
  * In this case, no transaction is done

## Solution 1: DP


### Complexity
* Time: O(n)
* Space: O(n)，dp数组。这个数组无法再缩减了，因为最后要把所有的 `profit = Math.max(left[i] + right[i], profit)` 这么loop一遍

### Java
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        if (k <= 0) {
            return 0;
        }
        
        int n = prices.length;
        
        // if k >= n/2, 就相当于无限次的交易机会
        if (k >= n / 2) {
            int maxProfit = 0;
            for (int i = 1; i < n; i++) { 
                if (prices[i] > prices[i - 1]) {
                    maxProfit += prices[i] - prices[i - 1];
                }
            }
            return maxProfit;
        }
        
        int[][] dp = new int[n][k + 1];
        
        //for (int i = 1; i < n; i++) {
            //for (int j = 1; j <= k; j++) {
        for (int j = 1; j <= k; j++) {
            for (int i = 1; i < n; i++) {
                dp[i][j] = dp[i - 1][j];
                for (int b = 0; b <= i - 1; b++) { // "b" means "buy date"
                    if (prices[i] > prices[b]) {
                        dp[i][j] = Math.max(dp[i][j], dp[b][j - 1] + (prices[i] - prices[b]));
                    }
                }
            }
        }
        return dp[n - 1][k];
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock IV [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

{% include links.html %}
