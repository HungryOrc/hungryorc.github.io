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
* `int dp[][] = new int[number of items][maxTotalSize + 1][maxTotalWeight + 1]`
* Base Cases
  * 对于数组里的第一个item
    * if(sizes[0] <= maxSize && weights[0] <= maxWeight)，`dp[0][sizes[0]][weights[0]] = values[0]`
  * 总size为0的情况，对于任何多个items，都必然是什么都不放，所以总value必是0。即 `dp[i][0] = 0`, 0 <= i < n。因为都是0，所以可以不写了
* Induction Rule: `dp[i][s][w] = max(dp[i - 1][s][w], dp[i - 1][s - sizes[i]][w - weights[i]] + values[i])`
* Return: 这里不是返回 `dp[n - 1][maxSize][maxWeight]`，因为达到maxTotalVal的时候未必是size或weight满。
正解是 `maxTotalValue = max(dp[n - 1][s][w])`，for all possible s and w

### Complexity
* Time: O(n * maxTotalSize * maxTotalWeight), 其中n是items的个数
* Space: O(n * maxTotalSize * maxTotalWeight)。可以优化为 O(maxTotalSize * maxTotalWeight)，因为dp矩阵里，第i行只用到第i-1行


<==== 到这里了！！！！


### Java
```java
public class Solution {
    public int backPackIII(int[] sizes, int[] values, int capacity) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0
                || sizes.length != values.length || capacity <= 0) {
            // 特殊情况别忘了：两个数组长度不同!
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case 1
        for (int i = 1; i <= capacity / sizes[0]; i++) {
            dp[0][sizes[0] * i] = values[0] * i;
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curValue = values[i];
            
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (curSize <= j) {
                    dp[i][j] = Math.max(dp[i][j], dp[i][j - curSize] + curValue);
                }
            }
        }
        
        int maxValue = 0;
        for (int v : dp[n - 1]) {
            maxValue = Math.max(maxValue, v);
        }
        return maxValue;
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

## Solution 2：一种很有趣的DFS方法。但速度超时 hoho <==== 我写的这个解法对么？

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
public class Solution {
    public int backPackIII(int[] sizes, int[] values, int capacity) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0
                || sizes.length != values.length || capacity <= 0) {
            return 0;
        }
        
        return dfs(sizes, values, 0, capacity, 0);
    }
    
    private int dfs(int[] sizes, int[] values, int curInd,
            int remainSize, int curTotalVal) {
        if (remainSize == 0) {
            return curTotalVal;
        } else if (remainSize < 0) {
            return 0; // 表示此路失败
        }
        
        if (curInd == sizes.length) {
            return curTotalVal;
        }
        
        int totalVal = 0;
        int curSize = sizes[curInd];
        int curVal = values[curInd];
        for (int i = 0; i <= remainSize / curSize; i++) {
            totalVal = Math.max(totalVal,
                dfs(sizes, values, curInd + 1, remainSize - i * curSize, curTotalVal + i * curVal));
        }
        return totalVal;
    }
}
```

## Reference
[Backpack III [LintCode]](https://www.lintcode.com/problem/backpack-iii/description)

{% include links.html %}
