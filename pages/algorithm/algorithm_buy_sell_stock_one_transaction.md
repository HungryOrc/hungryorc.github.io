---
title: "Best Time to Buy and Sell Stock: at Most One Transaction"
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

### Follow up
初始的array很长，放不下一个机器，必须放不同机器怎么办

做法：每个机器做相同算法，然后每个机器的范围内找min和max，然后把这些 (min, max) pairs 连成一个新的array，再对这个新的array做相同的算法处理。注意每个机器里的min要排在max的前面。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) return 0;
        
        int min = prices[0];
        // int max = 0;
        int maxProfit = 0;
        int n = prices.length;
        
        for (int i = 1; i < n; i++) {
            int cur = prices[i];
            if (cur < min) {
                min = cur;
            } 
            // 关键点：这里的else语句 不要再拿cur和min或者max来比了！
            // 要直接比较 cur - min 和 当前的maxProfit 的关系！
            // 下面这样处理还有个好处是，可以杜绝 买价在卖价后面出现 的可能性
            else if (cur - min > maxProfit) {
                // max = cur;
                maxProfit = cur - min;
            }
        }

        return maxProfit;
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

{% include links.html %}
