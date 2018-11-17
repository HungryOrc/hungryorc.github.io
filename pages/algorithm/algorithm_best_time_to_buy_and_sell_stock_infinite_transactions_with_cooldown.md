---
title: "Best Time to Buy and Sell Stock: Infinite Transactions with One Day Cooldown after Each Sell"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_best_time_to_buy_and_sell_stock_infinite_transactions_with_cooldown.html
folder: algorithm
toc: false
---

## Description
Say you have an array for which the i-th element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. 
You may complete **as many transactions as you like** (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
* **After you sell your stock, you cannot buy stock on next day**. (ie, cooldown 1 day)

### Example
* Input: [1,2,3,0,2]
  * Output: 3
  * [buy, sell, cooldown, buy, sell]
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0]
  * Output: 0
  * In this case, no transaction is done

## Solution: DP，一次过

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        
        int n = prices.length;
        
        // 今天结束时持仓，历史累计损益
        int hold = 0;
        // 昨天结束时持仓，历史累计损益
        int holdPrev = 0;
        // 今天结束时空仓，历史累计损益
        int empty = 0;
        // 昨天结束时空仓，历史累计损益
        int emptyPrev = 0;
        // 前天结束时空仓，历史累计损益
        int emptyPrevPrev = 0;
        
        if (n >= 1) {
            hold = -1 * prices[0];
            empty = 0;
        }
        
        if (n >= 2) {
            holdPrev = hold;
            emptyPrev = empty;
            
            hold = Math.max(holdPrev, -1 * prices[1]);
            empty = Math.max(emptyPrev, holdPrev + prices[1]);
        }
        
        if (n >= 3) {
            for (int i = 2; i < n; i++) {
                emptyPrevPrev = emptyPrev;
                emptyPrev = empty;
                holdPrev = hold;
                
                empty = Math.max(emptyPrev, holdPrev + prices[i]);
                hold = Math.max(holdPrev, emptyPrevPrev - prices[i]);
            }
        }
        
        // 最后一定是空仓比持仓更有利润。因为持仓毕竟是付出了买入成本，
        // 最差的情况是到了最后一天，无论如何价格，都卖出，
        // 这样也比持仓过最后一天要好
        return empty;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock with Cooldown [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

{% include links.html %}
