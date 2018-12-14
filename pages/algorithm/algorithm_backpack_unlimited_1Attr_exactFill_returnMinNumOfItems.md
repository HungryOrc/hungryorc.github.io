---
title: "Backpack: Unlimited, One Attribute, Exactly Fill the Pack, Return Number of Solutions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_unlimited_1Attr_exactFill_returnMinNumOfItems.html
folder: algorithm
toc: false
---

## Description
Unlimited 的意思是 每个item可以被取用 0次到无限次。每个item只有一个属性，比如size或weight。背包有最大的容量（对于size或weight）。
求最少用多少个items，可以正好填满这个背包

### Example
略

## Solution 1: 二维DP
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可以取任意多次。最少用多少个item，能正好组成总 size = s。。
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases
  * 对于数组里的第一个item
    * if (sizes[0] <= capacity)，dp[0][sizes[0] * i] = i，这里i的意思是i个（这种）item，其中 i = 1, 2, 3...
    * 其他的 dp[0][s != sizes[0]] 都 = -1，因为不可能实现，用-1来表示不可能（也可以用Integer.MAX_VALUE）
  * 总size为0的情况，对于任何多个items，都是可以的，即什么都不放。这些情况都算是0个item。所以 `dp[i][0] = 0`, 0 <= i < n。因为都是0所以可以不写了
* Induction Rule: `dp[i][sum] = min(dp[i - 1][sum], dp[i][sum - curValue] + 1)`
  * `dp[i - 1][sum]` 表示 sum里面将没有item i 的任何参与
  * `dp[i][sum - curValue]` 表示 sum里面将存在item i 的一次或者多次参与
  * 注意，`dp[i][sum - curValue]` 是**包含了** `dp[i - 1][sum - curValue]` 的。所以上面的式子不再加入 `dp[i - 1][sum - curValue]`。理由是：
    * `dp[i - 1][sum - curValue]` 表示 item i **已经被使用0次**，然后将会被使用一次
    * `dp[i][sum - curValue]` 表示 item i **已经被使用 0次，1次，或多次了**，然后将再被使用一次。所以可见它是涵盖了上面那个情况的
* Return: `dp[n - 1][capacity of backpack]`

### Complexity
* Time: O(n * capacity), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPack_UnknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity < 0) {
            return 0;
        }
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case 1
        for (int i = 1; i <= capacity; i++) { // 注意i从1开始，因为i=0即总size为0是可以实现的
            dp[0][i] = -1; // 初始化都设为-1表示不可能实现的情况，例外是可以实现的情况，在下面的for loop里写
        }
        for (int i = 1; i <= capacity / sizes[0]; i++) {
            dp[0][sizes[0] * i] = i; // 这里是 = i，不是 = 1 了
        }
        
        // base case 2，可以不写
        //for (int i = 0; i < n; i++) {
        //    dp[i][0] = 0;
        //}
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int sum = 1; sum <= capacity; sum++) {
                dp[i][sum] = dp[i - 1][sum];
                
                // 别忘了判断 dp[i][sum - curSize] != -1
                if (sum >= curSize && dp[i][sum - curSize] != -1) {
                    dp[i][sum] = Math.min(dp[i][sum], dp[i][sum - curSize] + 1);
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
    public int backPack_UnknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity < 0) {
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
        if (sizes == null || sizes.length == 0 || capacity < 0) {
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


## Solution 2：一种很有趣的DFS方法。但速度超时 hoho

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
class Solution {
    public int backPack_UnknownProblemNumber(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity < 0) {
            return 0;        
        }
        
        return numOfWays(sizes, 0, capacity);
    }
    
    private int numOfWays(int[] sizes, int curIndex, int remain) {
        if (remain == 0) { // 这个条件要写在 curIndex == sizes.length 之前！否则会漏解！
            return 1;
        } else if (remain < 0) {
            return 0;
        }
        
        if (curIndex == sizes.length) {
            return 0;
        }
        
        int curSize = sizes[curIndex];
        
        int numWays = 0;
        for (int i = 0; i <= remain / curSize; i++) {
            numWays += numOfWays(sizes, curIndex + 1, remain - i * curSize);
        }
        return numWays;
    } 
}
```

## Reference
网上没找到这题

{% include links.html %}
