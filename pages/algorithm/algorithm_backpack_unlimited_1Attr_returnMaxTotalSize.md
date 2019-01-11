---
title: "Backpack: Unlimited, One Attribute, Return Max Total Size"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_unlimited_1Attr_returnMaxTotalSize.html
folder: algorithm
toc: false
---

## Description
一共有n元钱。商品有3种，价钱分别是150，250，350一个，每种商品都有无数个。n元钱随意买，但是买剩下的钱必须交给黑社会当保护费。所以就是尽量多花光。求最少剩下多少钱

这题其实相当于，三种物品，每种都有size值，都可以取无限次，总size有cap，求最大的总size

### Example
略

## Solution 1: 二维DP，速度前 20%
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可以取任意多次。在总size <= s 的前提下，最大的总size是多少
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases：见下面代码
* Induction Rule：见下面代码
* Return: max(dp[n - 1][i])

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPackX(int totalMoney) {
        // ignore validation
        
        int[] prices = {150, 250, 350};
        int n = 3; // n 是商品的种类数
        
        int[][] dp = new int[n][totalMoney + 1];
        
        // base case
        for (int i = 1; i <= totalMoney / prices[0]; i++) {
            dp[0][i * prices[0]] = i * prices[0];
        }
        
        // 从第二种商品开始
        for (int i = 1; i < n; i++) {
            int curPrice = prices[i];
            
            for (int j = 1; j <= totalMoney; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (j >= curPrice) {
                    dp[i][j] = Math.max(dp[i][j],
                                        dp[i - 1][j - curPrice] + curPrice);
                }
            }
        }
        
        int maxTotalSpent = 0;
        for (int totalSpent : dp[n - 1]) {
            maxTotalSpent = Math.max(maxTotalSpent, totalSpent);
        }
        return totalMoney - maxTotalSpent;
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！但是这题反而不能从大到小，还是得从小到大！所以DP矩阵降维不一定需要从大到小

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
代码比最初的方法简单了一些。但实测速度没啥变化其实。每一题不一样，有的题降维之后运算速度也能明显提升
```java
public class Solution {
    public int backPackX(int totalMoney) {
        // ignore validation
        
        int[] prices = {150, 250, 350};
        int n = 3; // n 是商品的种类数
        
        int[] dp = new int[totalMoney + 1]; // 一维

        for (int i = 1; i <= totalMoney / prices[0]; i++) {
            dp[i * prices[0]] = i * prices[0]; // 一维
        }
        
        for (int i = 1; i < n; i++) {
            int curPrice = prices[i];
            
            for (int j = curPrice; j <= totalMoney; j++) { // 一维
                dp[j] = Math.max(dp[j],
                                 dp[j - curPrice] + curPrice);
            }
        }
        
        int maxTotalSpent = 0;
        for (int totalSpent : dp) { // 一维
            maxTotalSpent = Math.max(maxTotalSpent, totalSpent);
        }
        return totalMoney - maxTotalSpent;
    }
}
```

## Reference
* [Backpack X [LintCode]](https://www.lintcode.com/problem/backpack-x/description)

{% include links.html %}
