---
title: "Backpack: 0 or 1, One Attribute, Return Max Total Size"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_01_1Attr_returnMaxTotalSize.html
folder: algorithm
toc: false
---

## Description
这一类题也叫 “Naive 0/1背包问题”，因为每个item最多取一次，Naive的意思是 每个item只有一个属性，比如size或weight。背包有最大的容量（对于size或weight）。
求给定最高capacity限制，最多能装到多满

### Example
略

## Solution 1：二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个，在总size不超过 s 的前提下，能装到的最大总size。
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases
  * 对于数组里的第一个item，if(sizes[0] <= capacity)，`dp[0][i] = sizes[0]`, for sizes[0] <= i <= capacity
    * 注意这个base case稍微特殊一点，它是从一点以后开始及其右边都是一个定值
  * size和为0的情况，对于任何多个items，都是可以的，即什么都不放。所以 `dp[i][0]` = 0, for 0 <= i < n。因为是0，所以也可以不写了
* Induction Rule
  * `dp[i][s] = max(dp[i - 1][s], dp[i - 1][s - sizes[i]] + sizes[i])`
    * `dp[i - 1][s]`：没有 item i 的参与
    * `dp[i - 1][s - sizes[i]] + sizes[i]`：有 item i 的参与
* Return: `dp[n - 1][capacity of backpack]`

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPack(int capacity, int[] sizes) {
        if (capacity <= 0 || sizes == null || sizes.length == 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        if (sizes[0] <= capacity) {
            for (int i = sizes[0]; i <= capacity; i++) {
                dp[0][i] = sizes[0];
            }
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (curSize <= j) {
                    dp[i][j] = Math.max(dp[i][j], 
                        dp[i - 1][j - curSize] + curSize);
                }
            }
        }
        return dp[n - 1][capacity];
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法，简化代码。实际运算速度比Solution 1 快了一点
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: 与Solution 1 相同
* Space: 与Solution 1 相同

### Java
```java
public class Solution {
    public int backPack(int capacity, int[] sizes) {
        if (capacity <= 0 || sizes == null || sizes.length == 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n -> n + 1
        
        // base case 1 不用写了
        
        for (int i = 1; i <= n; i++) { // i < n -> i <= n
            int curSize = sizes[i - 1]; // sizes[i] -> sizes[i - 1]
            
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (curSize <= j) {
                    dp[i][j] = Math.max(dp[i][j], 
                        dp[i - 1][j - curSize] + curSize);
                }
            }
        }
        return dp[n][capacity]; // dp[n - 1][...] -> dp[n][...]
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]。实测速度前15%，比Solution 1 快了很多

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！
这题如果从小往大写会出错，因为每个item只能用1次，而根据induction rule，如果前面已经把item i 用过一次了，后面item i 并不知道，当填写dp矩阵到第二维的总size更大的地方，会因为总size能容下第二个（以及更多个）item i 了，就再加入新的 item i。

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {
    public int backPack(int capacity, int[] sizes) {
        if (capacity <= 0 || sizes == null || sizes.length == 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 一维
        
        if (sizes[0] <= capacity) {
            for (int i = sizes[0]; i <= capacity; i++) {
                dp[i] = sizes[0]; // 一维
            }
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int j = capacity; j >= 1; j--) {
                // dp[j] = dp[j]; // 这里一维就没意义再写了
                
                if (curSize <= j) {
                    dp[j] = Math.max(dp[j], dp[j - curSize] + curSize);
                }
            }
        }
        return dp[capacity];
    }
}
```

## Solution 2：二维DP，另一种做法
* `boolean dp[i][s]`：使用数组里 index为0到i 的items中的任意几个，能否正好组成总size = s
* `boolean dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases
  * 对于数组里的第一个item，if(sizes[0] <= capacity)，`dp[0][sizes[0]] = true`
  * size和为0的情况，对于任何多个items，都是可以的，即什么都不放。所以 `dp[i][0] = true`, for 0 <= i < n
* Induction Rule
  * `dp[i][s] = dp[i - 1][s] || dp[i - 1][s - sizes[i]]`, 第二项仅当 sizes[i] <= s 时才存在
    * `dp[i - 1][s]`：没有 item i 的参与
    * `dp[i - 1][s - sizes[i]] + sizes[i]`：有 item i 的参与
* Return: 对于所有的 `dp[n - 1][s]`，s从capacity开始往小走，到哪里为dp value为true即返回那个s值

### Complexity
* Time: O(n * capacity)
* Space: O(n * capacity)

### Java
```java
public class Solution {
    public int backPack(int capacity, int[] sizes) {
        if (capacity <= 0 || sizes == null || sizes.length == 0) {
            return 0;
        }
        
        int n = sizes.length;
        boolean[][] dp = new boolean[n][capacity + 1];
        
        // base case 1
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }
        
        // base case 2
        if (sizes[0] <= capacity) {
            dp[0][sizes[0]] = true;
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                // 如果dp[i][j]已经是true了，就没必要改了
                if (curSize <= j && dp[i][j] == false) {
                    dp[i][j] = dp[i - 1][j - curSize];
                }
            }
        }
        
        for (int i = capacity; i >= 1; i--) {
            if (dp[n - 1][i]) {
                return i;
            }
        }
        return 0;
    }
}
```

## Solution 2.1：基于Solution 2，使用Offset One方法，简化代码。实际运算速度比Solution 2 快了一些

### Complexity
* Time: 与Solution 1 相同
* Space: 与Solution 1 相同

### Java
```java
public class Solution {
    public int backPack(int capacity, int[] sizes) {
        if (capacity <= 0 || sizes == null || sizes.length == 0) {
            return 0;
        }
        
        int n = sizes.length;
        boolean[][] dp = new boolean[n + 1][capacity + 1];
        
        // base case 1
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }
        
        // base case 2 不用写了
        
        for (int i = 1; i <= n; i++) { // i < n -> i <= n
            int curSize = sizes[i - 1]; // sizes[i] -> sizes[i - 1]
            
            for (int j = 1; j <= capacity; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (curSize <= j && dp[i][j] == false) {
                    dp[i][j] = dp[i - 1][j - curSize];
                }
            }
        }
        
        for (int i = capacity; i >= 1; i--) {
            if (dp[n][i]) { // dp[n - 1][i] -> dp[n][i]
                return i;
            }
        }
        return 0;
    }
}
```

## Solution 2.2：基于Solution 2，dp[index][size]降维为dp[size]。实测速度前15%，比Solution 2 快了很多

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！
这题如果从小往大写会出错，因为每个item只能用1次，而根据induction rule，如果前面已经把item i 用过一次了，后面item i 并不知道，当填写dp矩阵到第二维的总size更大的地方，会因为总size能容下第二个（以及更多个）item i 了，就再加入新的 item i。

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {
    public int backPack(int capacity, int[] sizes) {
        if (capacity <= 0 || sizes == null || sizes.length == 0) {
            return 0;
        }
        
        int n = sizes.length;
        boolean[] dp = new boolean[capacity + 1]; // 一维
        
        // base case 1
        dp[0] = true; // 一维
        
        
        // base case 2
        if (sizes[0] <= capacity) {
            dp[sizes[0]] = true; // 一维
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int j = capacity; j >= 1; j--) {
                // dp[j] = dp[j]; // 一维的情况下，这个也不用写了

                if (curSize <= j && dp[j] == false) {
                    dp[j] = dp[j - curSize];
                }
            }
        }
        
        for (int i = capacity; i >= 1; i--) {
            if (dp[i]) {
                return i;
            }
        }
        return 0;
    }
}
```

## Solution 3: DFS

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
class Solution {
    public int backPack_DidntFindThisQuestionOnline(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;
        }
        
        return minNumOfItems(sizes, 0, 0, capacity);
    }
    
    private int minNumOfItems(int[] sizes, int curIndex, int curNumItems, int remain) {
        if (remain == 0) { // 这个条件要写在 curIndex == sizes.length 之前！否则会漏解！
            return curNumOfItems;
        } else if (remain < 0) {
            return Integer.MAX_VALUE; // 表示未能正好装满，以失败结束
        }
        
        if (curIndex == sizes.length) {
            return Integer.MAX_VALUE; // 表示未能正好装满，以失败结束
        }
        
        // 不使用 current item
        int use = minNumOfItems(sizes, curIndex + 1, curNumItems, remain);
        // 使用 current item
        int notUse = minNumOfItems(sizes, curIndex + 1, curNumItems + 1, remain - sizes[curIndex]); 
        
        return Math.min(use, notUse);
    } 
}
```

## Reference
* [Backpack [LintCode]](https://www.lintcode.com/problem/backpack/description)

{% include links.html %}
