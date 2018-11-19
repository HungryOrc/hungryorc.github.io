---
title: "Best Time to Buy and Sell Stock: at Most K Transactions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_buy_sell_stock_k_transactions.html
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
### k >= n/2 的情况
我们可以单独特殊处理一下，以加快速度。当然也可以不特殊处理，不影响正确性。
* 如果 k >= n/2，那么意味着交易次数的限制不再有用，每天都可以交易，等同于k等于无限大
  * 如果买入当日不能卖出，那么上面的结论很容易理解
  * 如果买入当日可以卖出，那么上面的结论依然有效！因为一天的价格不变，买了又卖，相当于利润0，与不做交易是一样的，所以并没有消耗交易次数，那么事实上也就不受到k的限制
  * 如果卖了当天再买，这个是任何交易所都允许的，但同理，卖了当天再买也相当于当天没有交易，持仓过了一天，所以不占用总交易次数
* 那么我们就按照无限次交易的做法来做，那么就是只要 prices[i + 1] > prices[i]，我们就（虚拟）交易一次，即在i日买，在i+1日卖。
  * 这样逐日买卖的最大可能交易次数是O(n)，即O(n)次买和O(n)次卖，会超过k的限制，就算k>=n/2。但是按照上面说的，连续几日的当日买入卖出，其实相当于这几天都没有交易，都是持仓过去一整天不动

### k < n/2 的情况
就要老老实实用 二维DP 了
* `int[][] dp = new int[n][k + 1]`，`dp[i][j]`意味着 “截止到i日并包括i日，做小于等于j次的transaction（一买一卖算一个transaction），最多能得到多少利润”
* recursion rule: `dp[i][j] = max(dp[i - 1][j], dp[b][j - 1] + prices[i] - prices[b])`
  * `dp[i - 1][j]`是第i天没有交易，确切地说是第i天没有卖出的情况
  * `dp[b][j - 1] + prices[i] - prices[b]`是指第i天进行了一次卖出的情况。其中`b`是指i日之前的买入日，所以`0 <= b <= i - 1`
* 边界条件
  * dp[i][0] = 0，即0次transaction，当然利润是0. 既然结果是0，就不用去写代码了
  * dp[0][j] = 0，即第1天无论交易多少次，利润都是0，因为当日价格不变。既然结果是0，就不用去写代码了
* result为 `dp[n - 1][k]`

### Complexity
* Time: O(k * n^2)
* Space: O(k * n)，dp数组

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
        
        // 这里先loop日期，或者先loop交易次数，都是ok的
        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= k; j++) {
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

# Solution 2: DP 改进时间 kn^2 -> kn
基于上面的方法，改进时间复杂度。着眼点是 `for (int b = 0; b <= i - 1; b++)`这个循环，它的时间复杂度是n。它里面的的 `dp[b][j - 1] + (prices[i] - prices[b])` 之内，注意到 `prices[i]` 是不断变的，**而 `dp[b][j - 1] - prices[b]` 的最大值 在dp[][]的第二维从 `j - 1` 变成 `j` 以后就不再变了！**

所以我们可以用一个 **预处理的int array** 来记录 `j - 1` 时候的 `dp[b][j - 1] - prices[b]` 的最大值，即：
````java
preprocess[j - 1] = max(dp[b][j - 1] - prices[b])   // 0 <= b <= i - 1 
````
这样就可以免去每次跑这个loop。就可以把时间复杂度从 kn^2 降低到 kn。注意看下面代码里的标注。

* 其实这个预处理的dp数组，还可以进一步压缩为一个int变量！因为在这个数组里，每次都只用到j-1这个index，没有用到任何其他index！

* 我感觉空间复杂度应该还可以继续压缩！

### Complexity
* Time: O(k * n)
* Space: O(k * n)，dp数组

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
        int[] preprocess = new int[k + 1]; // 改动之处！加入 helper dp数组

        for (int j = 1; j <= k; j++) {
        
            // 下面这条语句是相当于 i = 0 的情况
            preprocess[j - 1] = dp[0][j - 1] - prices[0]; // 改动之处！
            // 因为 dp[0][x] 永远等于 0，所以上面等式的右半部分总是等于 - prices[0]
            
            for (int i = 1; i < n; i++) {                
                dp[i][j] = Math.max(dp[i - 1][j], prices[i] + preprocess[j - 1]);
                
                preprocess[j - 1] = Math.max(preprocess[j - 1], dp[i][j - 1] - prices[i]); // 改动之处！
            }
        }

        return dp[n - 1][k];
    }
}
```

## Reference
* [Best Time to Buy and Sell Stock IV [LeetCode]](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

{% include links.html %}
