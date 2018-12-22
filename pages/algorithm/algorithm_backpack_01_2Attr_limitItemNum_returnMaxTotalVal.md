---
title: "Backpack: 0 or 1, Two Attributes, Limited Number of Picked Items, Return Max Total Value"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_01_2Attr_limitItemNum_returnMaxTotalVal.html
folder: algorithm
toc: false
---

## Description
每个item 取0或1次，每个item有 2个属性：size和value。背包有最大的size容量。

这题的特殊之处在于：**最多取m个items**。求最大的总value。

### Example
略

## Solution 1：三维DP <----- 这个做法对么 ？？？？？
* `int dp[i][j][s]`：使用数组里 index为0到i 的items中的 **正好j个items**（j <= m），正好组成 总size = s，最大可能的 总value 是多少。
* `int dp[][][] = new int[number of items][m + 1][capacity of backpack + 1]`
* Base Cases
  * 对于index=0 的 item，if sizes[0] <= capacity, `dp[0][1][sizes[0]] = values[0]`
  * `dp[i][0][s] = 0`，因为item个数为0的话，总value自然为0。既然是0也就不用写出来了
  * `dp[i][j][0] = 0`，因为总size为0的话，总value自然为0。既然是0也就不用写出来了
* Induction Rule
  * `dp[i][j][s] = Math.max(dp[i - 1][j][s], dp[i - 1][j - 1][s - sizes[i]] + values[i])`
* Return: 注意，这里不是返回 `dp[n - 1][m][capacity]`。因为获得总value最大时，总items的个数未必是 m，总size也未必是 capacity。所以：
  * Return max(dp[n - 1][1 <= numItem <= m][1 <= totalSize <= capacity])

### Complexity
* Time: O(n * m * capacity), 其中n是items的个数
* Space: O(n * m * capacity)。可以优化为 O(m * capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPack_UnknownQuestionNumber(int capacity, int m, int[] sizes, int[] values) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0
                || sizes.length != values.length || capacity <= 0 || m <= 0) {
            return 0;
        }
    
        int n = sizes.length;
        int[][][] dp = new int[n][m + 1][capacity + 1];
        
        // base case 1：只取 index = 0 的 item
        if (sizes[0] <= capacity) {
            dp[0][1][sizes[0]] = values[0];
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curValue = values[i];
            
            // j表示正好使用了几个items。所以j至少等于1，最多等于i+1个（因为i是序号，比如序号0到4共有5个item）
            for (int j = 1; j <= i + 1 && j <= m; j++) {
            
                for (int sum = 1; sum <= capacity; sum++) {
                    dp[i][j][sum] = dp[i - 1][j][sum];
                    
                    if (sum - curSize >= 0) {
                        dp[i][j][sum] = Math.max(dp[i][j][sum], 
                                dp[i - 1][j - 1][sum - curSize] + curValue);
                    }
                }
            }
        }
        
        int maxTotalValue = 0;
        for (int sum = 1; sum <= capacity; sum++) {
            for (int j = 1; j <= m; j ++) {
                maxTotalValue = Math.max(maxTotalValue, dp[n - 1][j][sum]);
            }
        }
        return maxTotalValue;
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
...
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: 与Solution 1 相同
* Space: O(m * capacity)

### Java
```java
...
```

## Solution 2：DFS。速度超时 <----- 这个做法对么 ？？？？

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
class Solution {
    public int backPack_UnknownQuestionNumber(int capacity, int m, int[] sizes, int[] values) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0 ||
            sizes.length != values.length || capacity <= 0 || m <= 0) {
            return 0;
        }
        
        return maxTotalVal(sizes, values, 0, m, capacity, 0);
    }
    
    private int maxTotalVal(int[] sizes, int[] values, int curIndex, 
            int remainItemNum, int remainSize, int curTotalVal) {
        // 这个条件要写在第一个！针对remainItemNum还 >=0 但 remainSize 已经为负的情况
        if (remainSize < 0) { 
            return 0; // 失败
        }
        
        // 这个条件要写在 curIndex == sizes.length 之前！否则会漏解
        if (remainSize == 0 || remainItemNum == 0) { 
            return curTotalVal;
        }
        
        if (curIndex == sizes.length) {
            return 0; // 失败
        }
        
        int curSize = sizes[curIndex];
        int curVal = values[curIndex];
        
        int totalVal = 0;
        // 不使用 current item
        totalVal = Math.max(totalVal,
                            maxTotalVal(sizes, values, curIndex + 1, 
                                        remainItemNum, remainSize, curTotalVal);
        // 使用 current item
        totalVal = Math.max(totalVal,
                            maxTotalVal(sizes, values, curIndex + 1, 
                                        remainItemNum - 1, remainSize - curSize, curTotalVal + curVal); 
        return totalVal;
    } 
}
```

## Reference
网上没找到这题

{% include links.html %}
