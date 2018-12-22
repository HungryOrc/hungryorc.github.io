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
* Induction Rule: **这一题的核心关键就在这里了！外层loop当前的总size，内层loop物品的index！这个和一般的背包题的做法是相反的**
  ```java
  for (int s = 1; s <= capacity, s++) {
      for (int i = 1; i < sizes.length; i++) {
          dp[i][s] = dp[i - 1][s];
          if (sizes[i] <= s) {
              dp[i][s] += dp[i - 1][s - sizes[i]];
          }
      }
  }
  ```
  * 这个做法的道理是：
    * 相当于bag的capacity从小到大，然后，对于一定的capacity，对所有的items loop一遍，cur item可以有用和不用 两种情况。相当于对于每个capacity，每个item都来试一下，**关键是这个试是相当于把这个item放到（或者不用）当前这个半成品的items的permutation的   最   右   边！！** 
    * 然后对于每个capacity，每个item只能最多被装进去1次！虽然每个item都可以用无限次，但我们这么做就是为了 把permutation做出来。然后到了之后更大的capacity的时候，当前的这个item又会有一个机会，对于每次+1的capacity（所以总共来说每个item都有capacity次机会）。
* Return: `dp[n - 1][capacity]`

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(n)，因为dp矩阵里，第i行只用到第i-1行

### Java
```java
public class Solution {     
    public int backPack_unknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity < 0) {
            return 0;
        }
        // 如果背包容量是0，则 *有一种permutation！就是什么item也不取*
        if (capacity == 0) {
            return 1;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case
        for(int i = 1; i <= capacity / sizes[0]; i++) {
            dp[0][sizes[0] * i] = 1;
        }
        
        for (int s = 1; s <= capacity, s++) {
            for (int i = 1; i < n; i++) {
                dp[i][s] = dp[i - 1][s];
                if (sizes[i] <= s) {
                    dp[i][s] += dp[i - 1][s - sizes[i]];
                }
            }
        }
        return dp[n - 1][capacity];
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
    public int backPack_unknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity < 0) {
            return 0;
        }
        if (capacity == 0) {
            return 1;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n + 1
        
        // base case，这里也有所变化！
        for(int i = 0; i < n; i++) {
            dp[i][0] = 1;
        }
        
        for (int s = 1; s <= capacity, s++) {
            for (int i = 1; i <= n; i++) { // i <= n
                dp[i][s] = dp[i - 1][s];
                if (sizes[i] <= s) {
                    dp[i][s] += dp[i - 1][s - sizes[i]];
                }
            }
        }
        return dp[n][capacity]; // dp[n][...]
    }
}
```

## Solution 1.2：基于Solution 1，dp矩阵降维

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！但这一题和大部分背包都不是一个类型。它的填写方式不典型，可以算是一种从小到大填

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {     
    public int backPack_unknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity < 0) {
            return 0;
        }
        if (capacity == 0) {
            return 1;
        }
        
        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 一维
        
        // base case
        dp[0] = 1; // 一维，这里的dp[0]表示capacity为0的情况
        
        for (int s = 1; s <= capacity, s++) {
            for (int i = 0; i <= n; i++) { // i <= n
                // dp[s] = dp[s]; // 这一句在解法变成一维以后就没意义了
                if (sizes[i] <= s) {
                    dp[s] += dp[s - sizes[i]]; // 一维
                }
            }
        }
        return dp[capacity]; // 一维
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
