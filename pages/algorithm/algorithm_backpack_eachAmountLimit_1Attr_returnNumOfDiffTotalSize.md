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
* `boolean dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可能取用任意次（但这些次数必须符合每种item各自的次数限制），能否正好组成总 size = s
* `boolean dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases：见下面的代码
* Induction Rule: 见下面的代码
* Return: 在`dp[n - 1][i]`里，看有多少个true的

### Complexity
* Time: O(n * m * capacity)，n是有多少种items，m是平均每种item的允许个数
* Space: O(n * capacity)

### Java
```java
public class Solution {
    public int backPackVIII(int capacity, int[] sizes, int[] amounts) {
        // ignore validations
        
        int n = sizes.length;
        boolean[][] dp = new boolean[n][capacity + 1];
        
        // base case 1
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }
        
        // base case 2
        for (int i = 1; i <= capacity / sizes[0] && i <= amounts[0]; i++) {
            dp[0][i * sizes[0]] = true;
        }
        
        // 从第二个item开始
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curMaxAmount = amounts[i];
            
            // j=0即一个也不放的情况也要包含，不然 dp[i-1][k<curSize] 的这一段就
            // 无法被继承到 dp[i][k<curSize] 了
            for (int j = 0; j <= curMaxAmount; j++) {
                
                // 这里的做法不是一个一个铺cur item，而是本次放几个cur item 是首先就决定了的
                for (int k = j * curSize; k <= capacity; k++) {
                    if (dp[i - 1][k - j * curSize]) {
                        dp[i][k] = true;
                    }
                }
            }
        }
        
        int count = 0;
        // 这题要求有效的总size从1开始算，0不能算。但是0在前面的dp运算里必须设为true
        for (int i = 1; i <= capacity; i++) {
            if (dp[n - 1][i]) {
                count++;
            }
        }
        return count;
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
这题的代码简化并不明显。实测速度也没有提高
```java
public class Solution {
    public int backPackVIII(int capacity, int[] sizes, int[] amounts) {
        // ignore validations
        
        int n = sizes.length;
        boolean[][] dp = new boolean[n + 1][capacity + 1]; // n+1
        
        // base case 1 
        for (int i = 0; i <= n; i++) { // i<=n
            dp[i][0] = true;
        }
        
        // base case 2 就不用写了
        
        // 从第一个item开始
        for (int i = 1; i <= n; i++) { // i<=n
            int curSize = sizes[i - 1]; // i-1
            int curMaxAmount = amounts[i - 1]; // i-1
            
            for (int j = 0; j <= curMaxAmount; j++) {
                for (int k = j * curSize; k <= capacity; k++) {
                    if (dp[i - 1][k - j * curSize]) {
                        dp[i][k] = true;
                    }
                }
            }
        }
        
        int count = 0;
        for (int i = 1; i <= capacity; i++) {
            if (dp[n][i]) { // n
                count++;
            }
        }
        return count;
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
实测速度并没有什么提高
```java
public class Solution {
    public int backPackVIII(int capacity, int[] sizes, int[] amounts) {
        // ignore validations
        
        int n = sizes.length;
        boolean[] dp = new boolean[capacity + 1]; // 一维
        
        // base case 1，一维
        dp[0] = true;
        
        // base case 2，一维
        for (int i = 1; i <= capacity / sizes[0] && i <= amounts[0]; i++) {
            dp[i * sizes[0]] = true;
        }
        
        // 从第二个item开始
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            int curMaxAmount = amounts[i];
            
            // 一次一次地铺cur item，每次铺一个，一共铺 curMaxAmount 次
            for (int j = 1; j <= curMaxAmount && j <= capacity / curSize; j++) {
                
                // 从大到小铺，以避免重叠计算的错误
                for (int k = capacity; k >= 1; k--) {
                    if (k < j * curSize) {
                        break;
                    }
                    if (dp[k - curSize]) { // 一维
                        dp[k] = true;
                    }
                }
            }
        }
        
        int count = 0;
        for (int i = 1; i <= capacity; i++) {
            if (dp[i]) {
                count++;
            }
        }
        return count;
    }
}
```

## Reference
[Backpack VII [LintCode]](https://www.lintcode.com/problem/backpack-vii/description)

{% include links.html %}
