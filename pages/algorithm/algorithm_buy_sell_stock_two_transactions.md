---
title: "Best Time to Buy and Sell Stock: at Most Two Transactions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_buy_sell_stock_two_transactions.html
folder: algorithm
toc: false
---

## Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete **at most two transactions**.
一个`transaction`的意思是一次买加上一次卖。

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Example
* Input: [3,3,5,0,0,3,1,4]
  * Output: 6
  * Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3. Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
* Input: [1,2,3,4,5]
  * Output: 4
  * Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4. Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0]
  * Output: 0
  * In this case, no transaction is done

## Solution: DP
从左到右一个DP，从右到左一个DP

### Complexity
* Time: O(n)
* Space: O(n)，dp数组。这个数组无法再缩减了，因为最后要把所有的 `profit = Math.max(left[i] + right[i], profit)` 这么loop一遍

### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        
        // 在 i 以及 i的左边，进行一次交易，所能得到的最大利润
        int[] left = new int[prices.length];
        // 在 i 以及 i的右边，进行一次交易，所能得到的最大利润
        int[] right = new int[prices.length];

        // DP from left to right
        left[0] = 0;
        int min = prices[0];
        for (int i = 1; i < prices.length; i++) {
            min = Math.min(prices[i], min);
            left[i] = Math.max(left[i - 1], prices[i] - min);
        }

        // DP from right to left
        right[prices.length - 1] = 0;
        int max = prices[prices.length - 1];
        for (int i = prices.length - 2; i >= 0; i--) {
            max = Math.max(prices[i], max);
            right[i] = Math.max(right[i + 1], max - prices[i]);
        }

        int profit = 0;
        for (int i = 0; i < prices.length; i++){
            profit = Math.max(left[i] + right[i], profit);  
        }

        return profit;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock III [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)

{% include links.html %}
