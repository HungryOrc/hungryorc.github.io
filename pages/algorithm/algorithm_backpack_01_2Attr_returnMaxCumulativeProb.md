---
title: "Backpack: 01, Two Attributes, Return Max Cumulative Probability"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_01_2Attr_returnMaxCumulativeProb.html
folder: algorithm
toc: false
---

## Description
一共有一定数额的钱。要申请学校读书。有m个学校可以申请，每个学校都有2个属性：申请费，和录取概率。这些钱花不花完无所谓，要求的是 最高的 **至少被一所学校录取** 的概率

**至少被一所学校录取的概率，就是 1 - 所有学校都不录取的概率，后者是一个 连乘**

这题和限制总size，求最大的总value的题是同理的。区别只是在于这题的计算是乘法，不是加法

### Example
略

## Solution 1: 二维DP
* `double dp[i][money]`：申请数组里 index为0到i 的schools中的任意个school，每个school最多被申请一次。在总花费 <= money 的前提下，
**被所有申请的学校都拒绝 的最小概率是多少**
* `double dp[][] = new int[number of schools][money limit + 1]`
* Base Cases：见下面代码
* Induction Rule：见下面代码
* Return: max(dp[number of schools - 1][i])

### Complexity
* Time: O(n * moneyLimit), 其中n是学校的个数
* Space: O(n * moneyLimit)。可以优化为 O(moneyLimit)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public double backpackIX(int totalMoney, int[] prices, double[] probs) {
        // ignore validations
        
        int m = probs.length; // m is number of schools
        double[] failProbs = new double[m];
        for (int i = 0; i < m; i++) {
            failProbs[i] = 1 - probs[i];
        }
        
        // dp represents the MIN probability 
        // to fail ALL the schools that were applied
        double[][] dp = new double[m][totalMoney + 1];
        
        // 初始化，都设为1，fail概率为1即必败，这样就方便于之后再找min fail prob
        for (int i = 0; i < m; i++) {
            for (int j = 0; j <= totalMoney; j++) {
                dp[i][j] = 1;
            }
        }
        
        // base case, for the 1st school
        if (prices[0] <= totalMoney) {
            dp[0][prices[0]] = failProbs[0];
        }
        
        // 从第二个学校开始
        for (int i = 1; i < m; i++) {
            int curPrice = prices[i];
            double curFailProb = failProbs[i];
            
            for (int money = 1; money <= totalMoney; money++) {
                dp[i][money] = dp[i - 1][money];
                
                if (curPrice <= money) {
                    dp[i][money] = Math.min(dp[i][money], // 注意，这里是要取 min！
                                            dp[i - 1][money - curPrice] * curFailProb);
                }
            }
        }
        
        // min Prob To Fail All Schools That Were Applied
        double minFailProb = 1;
        for (double prob : dp[m - 1]) {
            minFailProb = Math.min(minFailProb, prob);
        }
        return 1 - minFailProb;
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]，速度比前面的方法 大幅提高

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {
    public double backpackIX(int totalMoney, int[] prices, double[] probs) {
        // ignore validations
        
        int m = probs.length; // m is number of schools
        double[] failProbs = new double[m];
        for (int i = 0; i < m; i++) {
            failProbs[i] = 1 - probs[i];
        }

        double[] dp = new double[totalMoney + 1]; // 一维

        for (int j = 0; j <= totalMoney; j++) {
            dp[j] = 1; // 一维
        }
        
        // base case, for the 1st school
        if (prices[0] <= totalMoney) {
            dp[prices[0]] = failProbs[0]; // 一维
        }
        
        // 从第二个学校开始
        for (int i = 1; i < m; i++) {
            int curPrice = prices[i];
            double curFailProb = failProbs[i];
            
            // 因为每个学校只能申请一次，所以这里必须 从大到小！
            for (int money = totalMoney; money >= curPrice; money--) { // 一维
                dp[money] = Math.min(dp[money],
                                     dp[money - curPrice] * curFailProb);
            }
        }

        double minFailProb = 1;
        for (double prob : dp) { // 一维
            minFailProb = Math.min(minFailProb, prob);
        }
        return 1 - minFailProb;
    }
}
```

## Reference
* [Backpack IX [LintCode]](https://www.lintcode.com/problem/backpack-ix/description)

{% include links.html %}
