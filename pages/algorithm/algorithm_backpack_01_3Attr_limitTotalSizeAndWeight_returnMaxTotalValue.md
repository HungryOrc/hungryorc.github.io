---
title: "Backpack: 0 or 1, Three Attributes, Total Size and Weight Limit, Return Max Total Value"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_01_3Attr_limitTotalSizeAndWeight_returnMaxTotalValue.html
folder: algorithm
toc: false
---

## Description
每个item可以被用 0到1次。每个item有3个属性，size, weight, value。背包有 total size limit 和 total weight limit。
求 max possible total value（达到 max value 时背包未必到达 size cap 或 weight cap）。

### Example
略

## Solution 1: 三维DP
* `int dp[i][s][w]`：使用数组里 index为0到i 的items中的任意几个item，每个item可以取0或1次，正好组成总 size = s，且总 weight = w 的前提下，
最大的总value是多少
* `int dp[][][] = new int[number of items][maxTotalSize + 1][maxTotalWeight + 1]`
* Base Cases
  * 对于数组里的第一个item
    * if(sizes[0] <= maxSize && weights[0] <= maxWeight)，`dp[0][sizes[0]][weights[0]] = values[0]`
  * 总size为0，或者总weight为0 的情况，对于任何多个items，都必然是什么都不放，所以总value必是0。因为都是0，所以可以不写了
* Induction Rule: `dp[i][s][w] = max(dp[i - 1][s][w], dp[i - 1][s - sizes[i]][w - weights[i]] + values[i])`
* Return: 这里不是返回 `dp[n - 1][maxSize][maxWeight]`，因为达到maxTotalVal的时候未必是size或weight满。
正解是 `maxTotalValue = max(dp[n - 1][s][w])`，for all possible s and w

### Complexity
* Time: O(n * maxTotalSize * maxTotalWeight), 其中n是items的个数
* Space: O(n * maxTotalSize * maxTotalWeight)。可以优化为 O(maxTotalSize * maxTotalWeight)，因为dp矩阵里，第i行只用到第i-1行

### Java
```java
public class Solution {     
    public int backPack_unknownProblemNumber(int maxTotalSize, int maxTotalWeight, 
            int[] sizes, int[] weights, int[] values) {
        if (maxTotalSize <= 0 || maxTotalWeight <= 0 ||
                sizes == null || weights == null || values == null ||
                sizes.length == 0 || weights.length == 0 || values.length == 0 ||
                sizes.length != weights.length || weights.length != values.length) {
            return 0;
        }
        
        int n = sizes.length;
        int[][][] dp = new int[n][maxTotalSize + 1][maxTotalWeight + 1];
        
        // base case
        if(sizes[0] <= maxTotalSize && weights[0] <= maxTotalWeight) {
            dp[0][sizes[0]][weights[0]] = values[0];
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curWeight = weights[i];
            int curValue = values[i];
            
            for (int sumS = 1; sumS <= maxTotalSize; sumS ++) {
                for (int sumW = 1; sumW <= maxTotalWeight; sumW ++) {
                
                    dp[i][sumS][sumW] = dp[i - 1][sumS][sumW];
                    
                    if (sumS - curSize >= 0 && sumW - curWeight >= 0) {
                        dp[i][sumS][sumW] = Math.max(
                            dp[i][sumS][sumW], dp[i - 1][sumS - curSize][sumW - curWeight] + curValue);
                    }
                }
            }
        }
        
        int max = 0;
        for (int sumS = 1; sumS <= maxTotalSize; sumS ++) {
            for (int sumW = 1; sumW <= maxTotalWeight; sumW ++) {
                max = Math.max(max, dp[n - 1][sumS][sumW]);
            }
        }
        return max;
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法。简化代码 <---- 这个写法还没有判断对错
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: 与Solution 1 相同
* Space: 与Solution 1 相同

### Java
```java
public class Solution {     
    public int backPack_unknownProblemNumber(int maxTotalSize, int maxTotalWeight, 
            int[] sizes, int[] weights, int[] values) {
        if (maxTotalSize <= 0 || maxTotalWeight <= 0 ||
                sizes == null || weights == null || values == null ||
                sizes.length == 0 || weights.length == 0 || values.length == 0 ||
                sizes.length != weights.length || weights.length != values.length) {
            return 0;
        }
        
        int n = sizes.length;
        int[][][] dp = new int[n + 1][maxTotalSize + 1][maxTotalWeight + 1]; // int[n + 1][...][...]
        
        // base case 不用写了
        
        for (int i = 1; i <= n; i++) { // i <= n
            int curSize = sizes[i - 1]; // 下面这三个都要从 [i] 变成 [i - 1]
            int curWeight = weights[i - 1];
            int curValue = values[i - 1];
            
            for (int sumS = 1; sumS <= maxTotalSize; sumS ++) {
                for (int sumW = 1; sumW <= maxTotalWeight; sumW ++) {
                
                    dp[i][sumS][sumW] = dp[i - 1][sumS][sumW];
                    
                    if (sumS - curSize >= 0 && sumW - curWeight >= 0) {
                        dp[i][sumS][sumW] = Math.max(
                            dp[i][sumS][sumW], dp[i - 1][sumS - curSize][sumW - curWeight] + curValue);
                    }
                }
            }
        }
        
        int max = 0;
        for (int sumS = 1; sumS <= maxTotalSize; sumS ++) {
            for (int sumW = 1; sumW <= maxTotalWeight; sumW ++) {
                max = Math.max(max, dp[n][sumS][sumW]); // dp[n][...][...]
            }
        }
        return max;
    }
}
```

## Solution 1.2：基于Solution 1，dp矩阵降维 <---- 这个写法还没有判断对错

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {     
    public int backPack_unknownProblemNumber(int maxTotalSize, int maxTotalWeight, 
            int[] sizes, int[] weights, int[] values) {
        if (maxTotalSize <= 0 || maxTotalWeight <= 0 ||
                sizes == null || weights == null || values == null ||
                sizes.length == 0 || weights.length == 0 || values.length == 0 ||
                sizes.length != weights.length || weights.length != values.length) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[maxTotalSize + 1][maxTotalWeight + 1]; // 降到二维
        
        // base case
        if(sizes[0] <= maxTotalSize && weights[0] <= maxTotalWeight) {
            dp[sizes[0]][weights[0]] = values[0]; // 降到二维
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curWeight = weights[i];
            int curValue = values[i];
            
            for (int sumS = maxTotalSize; sumS >= 1; sumS--) { // 从大到小！
                for (int sumW = maxTotalWeight; sumW >= 1; sumW--) { // 从大到小！
                
                    // dp[sumS][sumW] = dp[sumS][sumW]; // 降维以后这一句就没意义了
                    
                    if (sumS - curSize >= 0 && sumW - curWeight >= 0) {
                        dp[sumS][sumW] = Math.max(
                            dp[sumS][sumW], dp[sumS - curSize][sumW - curWeight] + curValue); // 降到二维
                    }
                }
            }
        }
        
        int max = 0;
        for (int sumS = 1; sumS <= maxTotalSize; sumS ++) {
            for (int sumW = 1; sumW <= maxTotalWeight; sumW ++) {
                max = Math.max(max, dp[sumS][sumW]); // 降到二维
            }
        }
        return max;
    }
}
```

## Solution 2：DFS <---- 这个写法还没有判断对错

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
