---
title: "Backpack: Unlimited, One Attribute, Exactly Fill the Pack, Return Number of Solutions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_unlimited_1Attr_exactFill_returnNumOfSolutions.html
folder: algorithm
toc: false
---

## Description
Unlimited 的意思是 每个item可以被取用 0次到无限次。
另外，每个item只有一个属性，比如size或weight。
背包有最大的容量（对于size或weight）。求一共有多少种不同的装载方法正好填满这个背包

### Example
略

## Solution 1：二维DP，速度前20%
* `int dp[i][s]`：使用数组里 index为0到i 的items中的任意几个item，每个item可以取任意多次，一共有多少种不同的组合方式，能正好组成总 size = s。
* `int dp[][] = new int[number of items][capacity of backpack + 1]`
* Base Cases
  * 对于数组里的第一个item
    * if (sizes[0] <= capacity)，dp[0][sizes[0] * i] = 1，这里1的意思是1种方法，其中 i = 1, 2, 3...
    * 其他的 dp[0][s != sizes[0]] 都 = 0，因为不可能实现，所以是0种方法
  * 总size为0的情况，对于任何多个items，都是可以的，即什么都不放，这算是1种方法。所以 `dp[i][0]` = 1, for 0 <= i < n
* Induction Rule
  `dp[i][size] += dp[i - 1][size - j * sizes[i]]`, `0 <= j <= size / sizes[i]`
  * 上面这个induction rule其实包含了2个方面：
  * `dp[i][size] += dp[i - 1][size]`
  * `dp[i][size] += dp[i - 1][size - j * sizes[i]]`, `1 <= j <= size / sizes[i]`
* Return: `dp[n - 1][capacity of backpack]`

### Complexity
* Time: O(n * capacity * (sum / curSize)), 其中n是items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {
    public int backPackIV(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;        
        }
        
        int n = sizes.length;
        int[][] dp = new int[n][capacity + 1];
        
        // base case 1
        for (int i = 1; i <= capacity / sizes[0]; i++) {
            dp[0][sizes[0] * i] = 1;
        }
        
        // base case 2
        for (int i = 0; i < n; i++) {
            dp[i][0] = 1;
        }
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];
            
            for (int sum = 1; sum <= capacity; sum++) {
                
                for (int j = 0; j <= sum / curSize; j++) {
                    dp[i][sum] += dp[i - 1][sum - j * curSize];
                }
            }
        }
        return dp[n - 1][capacity];
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法。对这题来说效果微弱
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: O(n * capacity * (sum / curSize))，与Solution 1 相同
* Space: O(n * capacity)，与Solution 1 相同

### Java
```java
public class Solution {
    public int backPackIV(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;        
        }
        
        int n = sizes.length;
        int[][] dp = new int[n + 1][capacity + 1]; // n -> n + 1
        
        // base case 1 不用了
        
        // base case 2
        for (int i = 0; i < n; i++) {
            dp[i][0] = 1;
        }
        
        for (int i = 1; i <= n; i++) { // i < n i <= n
            int curSize = sizes[i - 1]; // sizes[i] -> sizes[i - 1]
            
            for (int sum = 1; sum <= capacity; sum++) {
                
                for (int j = 0; j <= sum / curSize; j++) {
                    dp[i][sum] += dp[i - 1][sum - j * curSize];
                }
            }
        }
        return dp[n][capacity]; // dp[n - 1][capacity] -> dp[n][capacity]
    }
}
```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]。对这题来说效果微弱

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: O(n * capacity * (sum / curSize))，与Solution 1 相同，对这题来说，实测速度没有变化
* Space: O(capacity)

### Java
```java
public class Solution {
    public int backPackIV(int[] sizes, int capacity) {
        if (sizes == null || sizes.length == 0 || capacity <= 0) {
            return 0;        
        }
        
        int n = sizes.length;
        int[] dp = new int[capacity + 1]; // 去掉了第一维
        
        // base case 1
        for (int i = 1; i <= capacity / sizes[0]; i++) {
            dp[sizes[0] * i] = 1;
        }
        
        // base case 2
        dp[0] = 1; // 去掉了第一维
        
        for (int i = 1; i < n; i++) {
            int curSize = sizes[i];

            for (int sum = capacity; sum >= 1; sum--) { // 这一维的顺序改为从大到小！

                // 这一维不要改为从大到小！因为j从小到大的结果其实就是剩余的sum从大到小！
                // 另外还要注意，j要从1开始了！不要像以前一样从0开始！因为降维了，
                // 还从0开始就意味着 dp[sum] += dp[sum]，这样重复加是错的
                for (int j = 1; j <= sum / curSize; j++) {
                    dp[sum] += dp[sum - j * curSize];
                }
            }
        }
        return dp[capacity]; // 去掉了第一维
    }
}
```

## Solution 2: 二维DP，与Solution 1 同样的思路，衍生出另一种看起来不同其实同理的 Induction Rule。运行速度相同


// 这里分两种情况，
    // 加号左边 dp[i - 1][sum] 表示，sum里面将没有array[i]的任何参与，
    // 加号右边 dp[i][sum - curValue] 表示，sum里面将存在array[i]的一次或者多次参与，
 
    // dp[i][sum - curValue] 是包含了 dp[i - 1][sum - curValue] 的 ！！！ 所以下面的式子不要再加 dp[i - 1][sum - curValue] ！！！
    // 因为：
    // dp[i - 1][sum - curValue] 表示 array[i] 已经被使用0次，然后将会被使用且仅使用一次 ！！！
    // dp[i][sum - curValue] 表示 array[i] 已经被使用 0次，1次，或多次了，然后将再被使用一次 ！！！
    
    dp[i][sum] = dp[i - 1][sum] + dp[i][sum - curValue];
}

// 方法2：
public class Solution {
    
    public static long makeChange(int[] coins, int money) {
        if (coins == null || coins.length == 0 || money < 0) {
            return 0L;
        }
        
        int n = coins.length;
        long[][] dp = new long[n][money + 1];
        
        for (int i = 0; i < n; i++) {
            dp[i][0] = 1L;
        }
        
        if (coins[0] <= money) {
            int sum = coins[0];
            while (sum <= money) {
                dp[0][sum] = 1L;
                sum += coins[0];
            }
        }
        
        for (int i = 1; i < n; i++) {
            for (int sum = 1; sum <= money; sum++) {
                int curValue = coins[i];
                
                if (curValue > sum) {
                    dp[i][sum] = dp[i - 1][sum];
                } else {
                    dp[i][sum] = dp[i - 1][sum] + dp[i][sum - curValue];
                }
            }
        }
        
        return dp[n - 1][money];
    }
}



下面的sulution 3 还没归纳呢！！！



## Solution 3：一种很有趣的DFS方法。但速度超时 hoho

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
```java
class Solution {
    public int backPackV(int[] sizes, int capacity) {
        // ignore the validation of the parameters
        
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
        
        int numWays = 0;
        numWays += numOfWays(sizes, curIndex + 1, remain); // 不使用 current item
        numWays += numOfWays(sizes, curIndex + 1, remain - sizes[curIndex]); // 使用 current item
        return numWays;
    } 
}
```

## Reference
* [Backpack V [LintCode]](https://www.lintcode.com/problem/backpack-v/description)

{% include links.html %}
