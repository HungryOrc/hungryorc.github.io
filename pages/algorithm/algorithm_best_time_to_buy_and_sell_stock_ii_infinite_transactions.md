---
title: "Best Time to Buy and Sell Stock: Infinite Transactions"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_best_time_to_buy_and_sell_stock_ii_infinite_transactions.html
folder: algorithm
toc: false
---

## Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete **infinite transactions**.
一个`transaction`的意思是一次买加上一次卖。

However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again). */

### Example
* Input: [7, 1, 5, 3, 6, 14]
  * Output: (5 - 1) + (6 - 3) + (14 - 6) = 15
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0]
  * Output: 0
  * In this case, no transaction is done

## Solution: 一次过
只要是第二天比第一天高，就认为前一天买了，第二天卖了，就加到总利润里去

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                maxProfit += (prices[i] - prices[i - 1]);
            }
        }
        return maxProfit;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock II [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

{% include links.html %}
