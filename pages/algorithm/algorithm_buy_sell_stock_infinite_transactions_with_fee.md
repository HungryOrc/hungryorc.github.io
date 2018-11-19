---
title: "Best Time to Buy and Sell Stock: Infinite Transactions with Transaction Fee"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_buy_sell_stock_infinite_transactions_with_fee.html
folder: algorithm
toc: false
---

## Description
Your are given an array of integers `prices`, for which the i-th element is the price of a given stock on day i; 
and **a non-negative integer** `fee` representing a transaction fee. 
You need to pay the transaction fee once for each transaction. 一个`transaction`的意思是一次买加上一次卖.

You may complete **infinite transactions**. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)

Return the maximum profit you can make.
* 0 < prices.length <= 50000
* 0 < prices[i] < 50000
* 0 <= fee < 50000
* 这样的量级就别想DFS了

### Example
* Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
  * Output: 8
  * The maximum profit can be achieved by:
    * Buying at prices[0] = 1
    * Selling at prices[3] = 8
    * Buying at prices[4] = 4
    * Selling at prices[5] = 9
    * The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
* 注意这个特殊情况！单调下降数组！Input: [11,7,5,4,1,0], fee = 1
  * Output: 0
  * In this case, no transaction is done

## Solution 1: DP，速度 前1%
和买卖股票有cooldown那题一样的思路，用2个dp array

* States (2 kinds)
  * 在第i日结束时持仓, the max possible profit at the end of day i is: `hold[i]`
  * 在第i日结束时空仓, the max possible profit at the end of day i is: `empty[i]`
* Induction Rules
  * `hold[i] = Math.max(hold[i - 1], empty[i - 1] - prices[i])`
  * `empty[i] = Math.max(empty[i - 1], hold[i - 1] + prices[i] - fee)`
* Result: `empty[prices.length - 1]`，因为无论最后一天的价格如何，盈亏如何，卖掉也比不卖好

### Complexity
* Time: O(n)
* Space: O(1)，由于 **状态 i 仅仅与状态 i-1 有关**，所以不必用一个长度n的数组来存状态

### Java
```java
public class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        
        int n = prices.length;
        int hold = 0, empty = 0;
        int holdPrev = 0, emptyPrev = 0;
        
        // 第一天的情况
        if (n >= 1) {
            hold = prices[0] * -1; // 按第一天的价格买入
            empty = 0; // 第一天什么也没做
        }
        
        // 从第二天开始，一直到世界的尽头
        if (n >= 2) {
            for (int i = 1; i < n; i++) {
                holdPrev = hold;
                emptyPrev = empty;
            
                hold = Math.max(holdPrev, emptyPrev - prices[i]);
                empty = Math.max(emptyPrev, holdPrev + prices[i] - fee);
            }
        }
        return empty;
    }
}
```

## Solution 1.1: 题目变种，buy和sell各有不同的fee。仍可用上面的DP思路
* States (2 kinds)，与上面的相同
  * 在第i日结束时持仓, the max possible profit at the end of day i is: `hold[i]`
  * 在第i日结束时空仓, the max possible profit at the end of day i is: `empty[i]`
* Induction Rules，有轻微区别，在下面标注了
  * `hold[i] = Math.max(hold[i - 1], empty[i - 1] - prices[i] - buyFee)`，区别在这里，要减去 buyFee
  * `empty[i] = Math.max(empty[i - 1], hold[i - 1] + prices[i] - sellFee)`
* Result: `empty[prices.length - 1]`，与上面的相同

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        
        int n = prices.length;
        int hold = 0, empty = 0;
        int holdPrev = 0, emptyPrev = 0;

        if (n >= 1) {
            hold = prices[0] * -1 - buyFee; // 区别在这里，减去buyFee
            empty = 0;
        }

        if (n >= 2) {
            for (int i = 1; i < n; i++) {
                holdPrev = hold;
                emptyPrev = empty;
            
                hold = Math.max(holdPrev, emptyPrev - prices[i] - buyFee); // 区别在这里，减去buyFee
                empty = Math.max(emptyPrev, holdPrev + prices[i] - sellFee);
            }
        }
        return empty;
    }
}
```

## Solution 2: 我自己的方法，不是DP，一次过，速度前10%
具体逻辑和日常交易原则关系很大。详见下面代码。逻辑并不复杂。实际代码也挺短。真想明白了就简单

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        
        int maxProfit = 0;
        int n = prices.length;
        int buy = prices[0];
        int sell = prices[0];
        
        for (int i = 1; i < n; i++) {
            int cur = prices[i];
            int prev = prices[i - 1];
            
            // 如果今天价格高于昨天价格，那么只需要看今天价格是否高于sell，
            // 如果是，则更新 sell = cur
            // 这种情况下不做卖出，卖出只是徒增手续费
            if (cur > prev) {
                if (cur > sell) {
                    sell = cur;
                } 
            } 
            // 如果今天价格低于昨天价格，那么考虑两件事：
            // 1. 如果sell>buy+fee，即卖出有利可图；且sell>cur+fee，即这个短差有价值，
            // 则卖出at sell，并重新买入at cur
            // 2. 否则，如果cur比buy还低（到了这里就意味着当前sell的话无利可图），
            // 则更新 buy=cur 且 sell=cur
            else if (cur < prev) {
                if (sell - buy > fee) { // 此时卖出有利可图
                    if (sell - cur > fee) { // 这个短差做的话有价值
                        maxProfit += sell - buy - fee;
                        buy = cur;
                        sell = cur;
                    }
                }
                // 此时卖出无利可图，且cur比buy还低了，那么就全部重新来过
                else if (cur < buy) {
                    buy = cur;
                    sell = cur;
                }
            }
        }
        
        // 切勿忘了这里！最后一笔一定还没被处理！而且也许值得做！
        if (sell - buy - fee > 0) {
            maxProfit += sell - buy - fee;
        }
        
        return maxProfit;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock with Transaction Fee [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

{% include links.html %}
