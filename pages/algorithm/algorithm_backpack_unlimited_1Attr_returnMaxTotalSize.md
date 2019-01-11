---
title: "Backpack: Unlimited, One Attribute, Return Max Total Size"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_unlimited_1Attr_returnMaxTotalSize.html
folder: algorithm
toc: false
---

## Description
一共有n元钱。商品有3种，价钱分别是150，250，350一个，每种商品都有无数个。n元钱随意买，但是买剩下的钱必须交给黑社会当保护费。所以就是尽量多花光

这题其实相当于，三种物品，每种都有size值，都可以取无限次，总size有cap，求最大的总size

### Example
略

## Solution 1: 二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可以取任意多次。在总size <= s 的前提下，最大的总size是多少
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases：见下面代码
* Induction Rule：见下面代码
* Return: max(dp[n - 1][i])

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPackX(int totalMoney) {
        // ignore validation
        
        int[] prices = {150, 250, 350};
        int n = 3; // n 是商品的种类数
        
        int[][] dp = new int[n][totalMoney + 1];
        
        // base case
        for (int i = 1; i <= totalMoney / prices[0]; i++) {
            dp[0][i * prices[0]] = i * prices[0];
        }
        
        // 从第二种商品开始
        for (int i = 1; i < n; i++) {
            int curPrice = prices[i];
            
            for (int j = 1; j <= totalMoney; j++) {
                dp[i][j] = dp[i - 1][j];
                
                if (j >= curPrice) {
                    dp[i][j] = Math.max(dp[i][j],
                                        dp[i - 1][j - curPrice] + curPrice);
                }
            }
        }
        
        int maxTotalSpent = 0;
        for (int totalSpent : dp[n - 1]) {
            maxTotalSpent = Math.max(maxTotalSpent, totalSpent);
        }
        return totalMoney - maxTotalSpent;
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
    public int backPack_UnknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n -> n + 1
        
        // base case 1，有了一定的简化。第一维是0的话，即“前0个"元素
        for (int i = 1; i <= capacity; i++) { // 注意i从1开始，因为i=0即总size为0是可以实现的，用0个元素实现
            dp[0][i] = -1; // 初始化都设为-1表示不可能实现的情况
        }
        // dp[0][0] = 0 是为以后留下的火种。不然dp[0][i]全都是-1的话，以后整个矩阵都会是-1，就没戏了
        
        for (int i = 1; i <= n; i++) { // i < n -> i <= n
            int curSize = sizes[i - 1]; // sizes[i] -> sizes[i - 1]
            
            for (int sum = 1; sum <= capacity; sum++) {
                dp[i][sum] = dp[i - 1][sum];
                
                // 别忘了判断 dp[i][sum - curSize] != -1
                if (sum >= curSize && dp[i][sum - curSize] != -1) {
                    dp[i][sum] = Math.min(dp[i][sum], dp[i][sum - curSize] + 1);
                }
            }
        }
        return dp[n][capacity]; // dp[n - 1][capacity] -> dp[n][capacity]
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！但是这题反而不能从大到小，还是得从小到大！所以DP矩阵降维不一定需要从大到小

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java
public class Solution {
    public int backPack_UnknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 去掉第一维
        
        // base case 1，去掉第一维
        for (int i = 1; i <= capacity; i++) {
            dp[i] = -1;
        }
        for (int i = 1; i <= capacity / sizes[0]; i++) {
            dp[sizes[0] * i] = i;
        }

        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int sum = 1; sum <= capacity; sum++) {
                // 去掉第一维以后，这个也就没意义了：dp[sum] = dp[sum];
                
                if (sum >= curSize && dp[s][sum - curSize] != -1) {
                    dp[sum] = Math.min(dp[sum], dp[sum - curSize] + 1);
                }
            }
        }
        return dp[capacity];
    }
}
```


## Solution 2：一种很有趣的DFS方法。但速度超时 hoho <==== 我写的这个解法对么？

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
class Solution {
    public int backPack_UnknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;        
        }
        
        return minNumItems(sizes, 0, 0, capacity);
    }
    
    private int minNumItems(int[] sizes, int curIndex, int curNumItems, int remain) {
        if (remain == 0) { // 这个条件要写在 curIndex == sizes.length 之前！否则会漏解！
            return curNumItems;
        } else if (remain < 0) {
            return Integer.MAX_VALUE; // 意味着失败
        }
        
        if (curIndex == sizes.length) {
            return Integer.MAX_VALUE; // 意味着失败
        }
        
        int curSize = sizes[curIndex];
        
        int numItems = Integer.MAX_VALUE;
        for (int i = 0; i <= remain / curSize; i++) { // 这里i要从0开始，表示一个也不用的情况
            // 注意，这里要有 curNumItems + i
            numItems = Math.min(numItems, minNumItems(
                sizes, curIndex + 1, curNumItems + i, remain - i * curSize));
        }
        return numItems;
    } 
}
```

## Reference
* [Backpack X [LintCode]](https://www.lintcode.com/problem/backpack-x/description)

{% include links.html %}
