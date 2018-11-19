---
title: "Best Time to Buy and Sell Stock: One Transaction"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_buy_sell_stock_one_transaction.html
folder: algorithm
toc: false
---

## Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete **at most one transaction**.
一个`transaction`的意思是一次买加上一次卖。

### Example
* Input: [7, 1, 5, 3, 6, 4]
  * Output: 5
  * max difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be later than buying price)
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0]
  * Output: 0
  * In this case, no transaction is done

## Solution: 一次过，速度 前1%
从左往右一个一个看这些数。留下当前所经过的最低的数optimalBuyPrice，记录它后面的所有数和它的差里最大的maxProfit。

如果出现比现在记录的最低数更低的数，则更新这个最低数。但不消除maxProfit，而是留着，继续与新的optimalBuyPrice之后的各个profits比大小

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        int lowest = Integer.MAX_VALUE;
        int maxProfit = 0;
        
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < lowest) {
                optimalBuyPrice = prices[i];
            } else if (prices[i] - lowest > maxProfit) {
                maxProfit = prices[i] - lowest;
            }
        }
        return maxProfit;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

{% include links.html %}
