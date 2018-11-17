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

解析题意：
* 这一题只考察最后的累计 profit，不管本金多少。然后只交易一种股票。最多持有一手，最少持有零手。
* 买入时，历史累计盈利要减去买入价。卖出时，历史累计盈利要加上卖出价。持仓时，股票的价格变动不影响盈利或任何其他东西。
* 一天之内重复买卖没有意义。因为股价在一天之内认为不变。买卖交易也没有成本。所以一天之内，只有一次操作是有意义的：
  * 今天开始时有持仓，今天结束时没有持仓，即今天卖了
  * 今天开始时没有持仓，今天结束时有持仓，即今天买了
  * 今天什么也没做。今天结束时的持仓状况与今天开始时的相同

### Example
* Input: [1,2,3,0,2]
  * Output: 3
  * [buy, sell, cooldown, buy, sell]
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0]
  * Output: 0
  * In this case, no transaction is done

## Solution: DP，我的方法，一次过
这题一看就应该用DP做。难点在于如何处理 “今天卖明天不能买”

大部分buy and sell stocks的题目，都是故意把一个buy和一个sell合成一个整体作为一个transaction来考虑，这样会更简单，分开不好做。
而这一题正好相反！得把buy和sell分开考虑才好做！捏在一起不好做！搞两个dp数组
* States 分成两种状态：
  * 在第i天结束时 有持仓，历史累计最大 profit 记为：`hold[i]`
  * 在第i天结束时 无持仓，历史累计最大 profit 记为：`empty[i]`
* Init: 第一天和第二天的情况
* Transition Functions:
  * `hold[i] = Math.max(hold[i - 1], empty[i - 2] - prices[i])`
    * 等式右边，第一项是昨天持仓今天没操作，第二项是昨天空仓今天买入了
  * `empty[i] = Math.max(empty[i - 1], hold[i - 1] + prices[i])`
    * 等式右边，第一项是昨天空仓今天没操作，第二项是昨天持仓今天卖出了
* 结果: `empty[prices.length - 1]`
  * 最后一定是空仓比持仓更有利润。因为持仓毕竟是付出了买入成本，最差的情况是到了最后一天，无论如何价格，都卖出，这样也比持仓过最后一天要好

空间的改进：对于这一题，由于**状态 i 仅仅与状态 i-1 及 状态 i-2 有关**，
所以不必用一个长度n的数组来存状态！只要 O(1) 的空间复杂度即可。详情见下面代码

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
            hold = -1 * prices[0]; // 第一天结束时有持仓，只能是第一天买入了
            empty = 0; // 第一天结束时空仓，只能是第一天什么也没做
        }
        
        if (n >= 2) {
            holdPrev = hold;
            emptyPrev = empty;
            
            // 第二天结束时有持仓，要么是第一天买了，要么是第二天买了
            hold = Math.max(holdPrev, -1 * prices[1]);
            // 第二天结束时空仓，要么是第一天没买第二天也没买，要么是第一天买了第二天卖了
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
        return empty;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock with Cooldown [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

{% include links.html %}
