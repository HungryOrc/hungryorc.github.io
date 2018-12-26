---
title: "Backpack: 0 or 1, Two Attributes, Limited Total Size, Return Max Total Value"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_01_2Attr_returnMaxTotalVal.html
folder: algorithm
toc: false
---

## Description
每个item 取0或1次，每个item有 2个属性：size和value。背包有最大的size容量。求最大的总value。

### Example
略

## Solution 1：二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的 任意个items，正好组成 总size = s，最大可能的 总value 是多少。
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases
  * 对于index=0 的 item，if sizes[0] <= capacity, `dp[0][sizes[0]] = values[0]`
  * `dp[i][0] = 0`，因为总size为0的话，总value自然为0。既然是0也就不用写出来了
* Induction Rule
  * `dp[i][s] = Math.max(dp[i - 1][s], dp[i - 1][s - sizes[i]] + values[i])`
* Return: 注意，这里不是返回 `dp[n - 1][capacity]`。因为获得总value最大时，总size未必是 capacity。所以：
  * Return max(dp[n - 1][1 <= totalSize <= capacity])

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPackII(int capacity, int[] sizes, int[] values) {
        if (capacity <= 0 || sizes == null || values == null ||
            sizes.length == 0 || values.length == 0 || sizes.length != values.length) {
            return 0;        
        }
        // 这里还考虑了sizes数组和values数组长度不同的情况！
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case 1
        if (sizes[0] <= capacity) {
            dp[0][sizes[0]] = values[0];
        }
        
        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (sizes[i] <= j) {
                    dp[i][j] = Math.max(dp[i][j], 
                                        dp[i - 1][j - sizes[i]] + values[i]);
                }
            }
        }
        
        int maxValue = 0;
        for (int value : dp[n - 1]) {
            maxValue = Math.max(maxValue, value);
        }
        return maxValue;
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法，简化代码
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: 与Solution 1 相同
* Space: 与Solution 1 相同

### Java
```java
public class Solution {
    public int backPackII(int capacity, int[] sizes, int[] values) {
        if (capacity <= 0 || sizes == null || values == null ||
            sizes.length == 0 || values.length == 0 || sizes.length != values.length) {
            return 0;        
        }

        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n + 1
        
        // base case 1, no need to write now
        
        for (int i = 1; i <= n; i++) { // i <= n
            int curSize = sizes[i - 1]; // i - 1
            int curValue = values[i - 1]; // i - 1
        
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (curSize <= j) {
                    dp[i][j] = Math.max(dp[i][j], 
                                        dp[i - 1][j - curSize] + curValue);
                }
            }
        }
        
        int maxValue = 0;
        for (int value : dp[n]) { // dp[n]
            maxValue = Math.max(maxValue, value);
        }
        return maxValue;
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {
    public int backPackII(int capacity, int[] sizes, int[] values) {
        if (capacity <= 0 || sizes == null || values == null ||
            sizes.length == 0 || values.length == 0 || sizes.length != values.length) {
            return 0;        
        }

        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 1 dimension
        
        // base case 1
        if (sizes[0] <= capacity) {
            dp[sizes[0]] = values[0]; // 1 dimension
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curValue = values[i];
            
            // this loop is changed to start at bigger and end at smaller!!
            for (int j = capacity; j >= 1; j--) {
                // dp[j] = dp[j]; // for 1 dimension, we don't need this now
                
                if (curSize <= j) {
                    dp[j] = Math.max(dp[j], 
                                     dp[j - curSize] + curValue); // 1 dimension
                }
            }
        }
        
        int maxValue = 0;
        for (int value : dp) { // 1 dimension
            maxValue = Math.max(maxValue, value);
        }
        return maxValue;
    }
}
```

## Solution 2：DFS。速度超时

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
public class Solution {
    public int backPackII(int capacity, int[] sizes, int[] values) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0 ||
            sizes.length != values.length || capacity <= 0) {
            return 0;
        }
        
        return maxTotalVal(sizes, values, 0, capacity, 0);
    }
    
    private int maxTotalVal(int[] sizes, int[] values, int curIndex, 
            int remainSize, int curTotalVal) {
        // 这个条件要写在第一个
        if (remainSize < 0) { 
            return 0; // 失败
        }
        
        if (remainSize == 0 || curIndex == sizes.length) { 
            return curTotalVal;
        }
        
        int curSize = sizes[curIndex];
        int curVal = values[curIndex];
        
        int totalVal = 0;
        // 不使用 current item
        totalVal = Math.max(totalVal,
                            maxTotalVal(sizes, values, curIndex + 1, 
                                        remainSize, curTotalVal));
        // 使用 current item
        totalVal = Math.max(totalVal,
                            maxTotalVal(sizes, values, curIndex + 1, 
                                        remainSize - curSize, curTotalVal + curVal)); 
        return totalVal;
    } 
}
```

## Reference
[Backpack II [LintCode]](https://www.lintcode.com/problem/backpack-ii/description)

{% include links.html %}
