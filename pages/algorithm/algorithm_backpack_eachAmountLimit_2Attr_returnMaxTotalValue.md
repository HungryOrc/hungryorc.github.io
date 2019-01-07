---
title: "Backpack: Each Kind of Item has Its Own Amount Limit, Two Attributes, Total Size Limit, Return Max Total Value"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_eachAmountLimit_2Attr_returnMaxTotalValue.html
folder: algorithm
toc: false
---

## Description
每种item 有各自的 最高取用次数。每个item有2个属性，size和value。背包有最大的容量（对于size）。
求 max possible total value（达到 max value 时背包未必被填满）。

### Example
略

## Solution 1: 二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可能取用任意次（但这些次数必须符合每种item各自的次数限制），正好组成总 size = s 的前提下，最大的总value是多少
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases：见下面的代码
* Induction Rule: 见下面的代码
* Return: 这里不是返回 `dp[n - 1][capacity]`。因为 获得总value最大时的总size未必是capacity。所以应该返回的是：`max(dp[n - 1][totalSize])`, for 1 <= totalSize <= capacity

### Complexity
* Time: O(n * capacity)，n 是有多少种 items
* Space: O(n * capacity)

### Java
```java
public class Solution {
    public int backPackVII(int capacity, int[] sizes, int[] values, int[] amounts) {
        if (capacity <= 0 || sizes == null || values == null || amounts == null ||
            sizes.length == 0 || values.length == 0 || amounts.length == 0 ||
            sizes.length != values.length || values.length != amounts.length) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case
        for (int i = 1; i <= amounts[0] && i * sizes[0] <= capacity; i++) {
            dp[0][i * sizes[0]] = i * values[0];
        }
        
        // 从第二种item开始
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curValue = values[i];
            int curMaxAmount = amounts[i];
            
            // 以下两
            
            // for every possible amount of the current item
            for (int j = 0; j <= curMaxAmount; j++) {
                
                // for every possible total capacity
                // k 必须 >= curSize * i，下面的运算才有意义，所以不如直接从
                // curSize * i 开始 loop k 的值
                for (int k = curSize * j; k <= capacity; k++) {
                    dp[i][k] = Math.max(dp[i][k], 
                                        dp[i - 1][k - curSize * j] + curValue * j);
                }
            }
        }
        
        int max = 0;
        for (int totalVal : dp[n - 1]) {
            max = Math.max(max, totalVal);
        }
        return max;
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法。简化代码
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: 与Solution 1 相同
* Space: 与Solution 1 相同

### Java
```java
public class Solution {
    public int backPackIII(int[] sizes, int[] values, int capacity) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0
                || sizes.length != values.length || capacity <= 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n + 1
        
        // base case 1 不必写了

        for (int i = 1; i <= n; i++) { // i <= n
            int curSize = sizes[i - 1]; // i - 1
            int curValue = values[i - 1]; // i - 1
            
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (curSize <= j) {
                    dp[i][j] = Math.max(dp[i][j], dp[i][j - curSize] + curValue);
                }
            }
        }
        
        int maxValue = 0;
        for (int v : dp[n]) { // dp[n]
            maxValue = Math.max(maxValue, v);
        }
        return maxValue;
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]，速度 前1% ！

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！但是这题反而不能从大到小，还是得从小到大！所以DP矩阵降维不一定需要从大到小

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {
    public int backPackIII(int[] sizes, int[] values, int capacity) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0
                || sizes.length != values.length || capacity <= 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 一维
        
        // base case 1
        for (int i = 1; i <= capacity / sizes[0]; i++) {
            dp[sizes[0] * i] = values[0] * i; // 一维
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curValue = values[i];
            
            for (int j = 1; j <= capacity; j++) {
                // dp[j] = dp[j]; // 在一维下，这个没意义了
                
                if (curSize <= j) {
                    dp[j] = Math.max(dp[j], dp[j - curSize] + curValue); // 一维
                }
            }
        }
        
        int maxValue = 0;
        for (int v : dp) { // 一维
            maxValue = Math.max(maxValue, v);
        }
        return maxValue;
    }
}
```

## Reference
[Backpack VII [LintCode]](https://www.lintcode.com/problem/backpack-iii/description)

{% include links.html %}
