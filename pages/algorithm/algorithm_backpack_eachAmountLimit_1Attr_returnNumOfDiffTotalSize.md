---
title: "Backpack: Each Kind of Item has Its Own Amount Limit, One Attribute, Total Size Limit, Return Number of Different Total Size"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_eachAmountLimit_1Attr_returnNumOfDiffTotalSize.html
folder: algorithm
toc: false
---

## Description
每种item 有各自的 最高取用次数。每个item有1个属性: size。背包有最大的容量（对于size）。
求这些items一共可以组成多少种不同的总size（这些总size都要小于size limit）。

这题的思路和代码，**可以对比 "每种item都有次数限制，每个item有2个属性，求最大总value" 那题**

### Example
略

## Solution 1: 二维DP
* `boolean dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可能取用任意次（但这些次数必须符合每种item各自的次数限制），正好组成总 size = s 的前提下，最大的总value是多少
* `boolean dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases：见下面的代码
* Induction Rule: 见下面的代码
* Return: 这里不是返回 `dp[n - 1][capacity]`。因为 获得总value最大时的总size未必是capacity。所以应该返回的是：`max(dp[n - 1][totalSize])`, for 1 <= totalSize <= capacity

### Complexity
* Time: O(n * m * capacity)，n是有多少种items，m是平均每种item的允许个数
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
            
            // 以下两层for loop 是这个解法的精华所在 ！！！
            
            // 这里的j表示 cur item 在整个组合里出现多少次！！！
            // 特别注意：这里 j 要从 0 开始！就是不放任何 cur item 的情况
            for (int j = 0; j <= curMaxAmount; j++) {
                
                // for every possible total size k
                // 因为 k 必须 >= curSize * i，下面的运算才有意义，所以不如直接从
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
    public int backPackVII(int capacity, int[] sizes, int[] values, int[] amounts) {
        if (capacity <= 0 || sizes == null || values == null || amounts == null ||
            sizes.length == 0 || values.length == 0 || amounts.length == 0 ||
            sizes.length != values.length || values.length != amounts.length) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n + 1
        
        // 从第一种item开始
        for (int i = 1; i <= n; i++) { // i = 1, i <= n
            int curSize = sizes[i - 1]; // 以下三行都从 i 变成 i-1
            int curValue = values[i - 1];
            int curMaxAmount = amounts[i - 1];

            for (int j = 0; j <= curMaxAmount; j++) {
                for (int k = curSize * j; k <= capacity; k++) {
                    dp[i][k] = Math.max(dp[i][k], 
                                        dp[i - 1][k - curSize * j] + curValue * j);
                }
            }
        }
        
        int max = 0;
        for (int totalVal : dp[n]) { // dp[n]
            max = Math.max(max, totalVal);
        }
        return max;
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！
这题必须 从大到小填写！因为每种item都有个数限制

如果每种物品都可以放无数个，那么 从小到大填写 是可以的，因为反正都是可以无限次取用，再怎么重复累计也都不会犯错

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

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
        int[] dp = new int[capacity + 1]; // 一维
        
        // base case，一维
        for (int i = 1; i <= amounts[0] && i * sizes[0] <= capacity; i++) {
            dp[i * sizes[0]] = i * values[0];
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curValue = values[i];
            int curMaxAmount = amounts[i];
            
            // 注意！这里的j的意义和前面那些方法里的j的意义完全不同！虽然看起来关于j的代码基本一样，
            // 但这个方法里的j表示的是 “我么一次又一次地铺cur item 到组合里去，每次铺一个 cur item，
            // 当前是第j次铺一个 cur item 到组合里去”
            for (int j = 1; j <= curMaxAmount; j++) {
                for (int k = capacity; k >= j * curSize; k--) {
                    dp[k] = Math.max(dp[k], 
                                     dp[k - curSize] + curValue); // 一维
                }
            }
        }
        
        int max = 0;
        for (int totalVal : dp) { // 一维
            max = Math.max(max, totalVal);
        }
        return max;
    }
}
```

## Reference
[Backpack VII [LintCode]](https://www.lintcode.com/problem/backpack-vii/description)

{% include links.html %}
