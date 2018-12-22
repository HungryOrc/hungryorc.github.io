---
title: "Backpack: Unlimited, Two Attributes, Exactly Fill Up, Return Number of Permutations of Items"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_unlimited_2Attr_exactFill_returnNumOfPermutation.html
folder: algorithm
toc: false
---

## Description
每个item可以被用 无限次。每个item有2个属性，size, value。每个item的size都是 > 0 的，且 **所有 item 的 size 不重复！这很重要！**。背包有 total size limit。
求恰好装满背包的话，有多少种不同的 **permutation**，就是说 **items 的排列顺序 matters！**

### Example
* Input: [1, 2, 4], bag capacity: 4
* Output: number of permutations of the items that exactly fill up the bag: 6
  * The 6 different permutations are: [1, 1, 1, 1], [1, 1, 2], [1, 2, 1], [2, 1, 1], [2, 2], [4]

## Solution 1: 二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可以取 任意次，正好组成总 size = s 的前提下，
有多少种permutaion of items
* `int dp[][][] = new int[number of items][capacity + 1]`
* Base Cases
  * 对于数组里的第一个item
    * `dp[0][sizes[0] * i] = 1`, for 1 <= i <= capacity / sizes[0]。因为都是第一个item自己一个在那里不断叠加，所以它们的permutation只有 1，比如 [1,1,1,1] 的permutation只是 1
  * 总size为0 的情况，对于任何多个items，都必然是什么都不放，所以permutation必是0。因为都是0，所以可以不写了
* Induction Rule: **这一题的核心关键就在这里了！**
  ```java
  
  ```
* Return: `dp[n - 1][capacity]`

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
public class Solution {
    public int backPack_unknownProblemNumber(int maxTotalSize, int maxTotalWeight, 
            int[] sizes, int[] weights, int[] values) {
        if (maxTotalSize <= 0 || maxTotalWeight <= 0 ||
                sizes == null || weights == null || values == null ||
                sizes.length == 0 || weights.length == 0 || values.length == 0 ||
                sizes.length != weights.length || weights.length != values.length) {
            return 0;
        }
        
        return dfs(sizes, weights, values, 0, maxTotalSize, maxTotalWeight, 0);
    }
    
    private int dfs(int[] sizes, int[] weights, int[] values, int curInd,
            int remainSize, int remainWeight, int curTotalVal) {
        if (remainSize < 0 || remainWeight < 0) {
            return 0; // 表示失败
        } else if (remainSize == 0 || remainWeight == 0) {
            return curTotalVal;
        }
        
        if (curInd == sizes.length) {
            return curTotalVal;
        }
        
        int totalVal = 0;
        int curSize = sizes[curInd];
        int curWeight = weights[curInd];
        int curVal = values[curInd];
        
        // 不用 cur item
        totalVal = Math.max(totalVal, 
            dfs(sizes, weights, values, curInd + 1, remainSize, remainWeight, curTotalVal));
        // 用 cur item
        totalVal = Math.max(totalVal,
            dfs(sizes, weights, values, curInd + 1, 
            remainSize - curSize, remainWeight - curWeight, curTotalVal + curVal));
        return totalVal;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
