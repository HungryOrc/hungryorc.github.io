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
求最少用多少个items，可以正好填满这个背包

### Example
略

## Solution 1：二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个，最少用多少个item，能正好组成总 size = s。
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases
  * 对于数组里的第一个item，if(sizes[0] <= capacity)，dp[0][sizes[0]] = 1; 其他的 dp[0][s != sizes[0]] 都 = -1，用-1来表示不可能（也可以用Integer.MAX_VALUE）
  * size和为0的情况，对于任何多个items，都是可以的，即什么都不放，即0个item。所以 `dp[i][0]` = 0, for 0 <= i < n。因为是0，所以也可以不写了
* Induction Rule
  * `dp[i][s] = min(dp[i - 1][s], dp[i - 1][s - sizes[i]] + 1)`
    * `dp[i - 1][s]`：i之前的那些items已经可以组成总和正好为 s 的组合了，item i 不参与的话，自然也还是和为s的组合
    * `dp[i - 1][s - sizes[i]]`：i之前的那些items组成了总和正好为 s - sizes[i] 的组合，那么 item i 参与进来后，自然正好就是和为s。但 item i 加入进来以后item个数就多了1，所以要 +1
* Return: `dp[n - 1][capacity of backpack]`

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
class Solution {
    public int backPack_DidntFindThisQuestionOnline(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;
        }
    
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case 1
        dp[0][0] = 0; // 这里要写0！因为要避免成为-1！这里是因为总size为0，那么当然只要0个item
        for (int i = 1; i < n; i++) {
            dp[0][i] = -1; // -1 表示不可能实现；但下面有个例外，见紧邻的下面那个if语句
        }
        if (sizes[0] <= capacity) {
            dp[0][sizes[0]] = 1;
        }

        for (int i = 1; i < n; i++) {
            int curItemSize = sizes[i];
            
            for (int sum = 1; sum <= capacity; sum++) {
                // case 1: item i 不参与
                // 下面这一句，以及前面的 dp[0][0] = 0，二者一起保证了对所有的i，永远有 dp[i][0] = 0 ！
                // 那么下面case 2 的 dp[i - 1][sum - curItemSize] + 1 在 sum == curItemSize 的时候也就会等于 1 ！
                dp[i][sum] = dp[i - 1][sum];
                
                // case 2: item i 参与
                if (sum >= curItemSize && dp[i - 1][sum - curItemSize] != -1) { // 别忘了检查是否是 -1 ！
                    dp[i][sum] = Math.min(dp[i][sum], dp[i - 1][sum - curItemSize] + 1);
                }
            }
        }
        return dp[n - 1][capacity];
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法，简化代码
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: O(n * capacity)，与Solution 1 相同
* Space: O(n * capacity)，与Solution 1 相同

### Java
```java
class Solution {
    public int backPack_DidntFindThisQuestionOnline(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // 第一维从n变为n+1
        
        // base case 1 不用写了，因为第一维是0的话，即“前0个"元素，自然永远item数是0了

        for (int i = 1; i <= n; i++) { // 从 i < n 变为 i <= n
            int curItemSize = sizes[i - 1]; // 从 i 变为 i - 1
            
            for (int sum = 1; sum <= capacity; sum++) {
                dp[i][sum] = dp[i - 1][sum];
                
                if (sum >= curItemSize && dp[i - 1][sum - curItemSize] != -1) {
                    dp[i][sum] = Math.min(dp[i][sum], dp[i - 1][sum - curItemSize] + 1);
                }
            }
        }
        return dp[n][capacity]; // 第一维从n-1变为n
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: O(n * capacity)，与Solution 1 相同。但其实空间下降以后，时间也下降了，只是不是量级意义上的下降
* Space: O(capacity)

### Java
```java
class Solution {
    public int backPack_DidntFindThisQuestionOnline(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 去掉了第一维（第一维原本size是n）
        
        // base case 1，去掉了第一维
        dp[0] = 0;
        for (int i = 1; i < n; i++) {
            dp[i] = -1;
        }
        if (sizes[0] <= capacity) {
            dp[sizes[0]] = 1;
        }
        
        for (int i = 1; i < n; i++) {
            int curItemSize = sizes[i];
            
            for (int sum = capacity; sum >= 0; sum--) { // 这里要变成从大往小循环！！
                // dp[sum] = dp[sum]; // 去掉了第一维。然后剩下的这一行也没意义了
                
                if (sum >= curItemSize && dp[sum - curItemSize] != -1) {
                    dp[sum] = Math.min(dp[sum], dp[sum - curItemSize] + 1); // 去掉了第一维
                }
            }
        }
        return dp[capacity]; // 去掉了第一维
    }
}
```

## Solution 2：一种很有趣的DFS方法。但速度超时 hoho <==== 我这样做对么？？？？

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
没有在网上找到这题

{% include links.html %}
