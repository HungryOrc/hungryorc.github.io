---
title: "Use Ones and Zeroes to Max Number of Form Strings"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_ones_and_zeroes_form_max_number_of_strings.html
folder: algorithm
toc: false
---

## Description
In the computer world, use restricted resource you have to generate maximum benefit is what we always want to pursue.

For now, suppose you are a dominator of m 0s and n 1s respectively. On the other hand, there is an array with strings consisting of only 0s and 1s.

Now your task is to find the maximum number of strings that you can form with given m 0s and n 1s. Each 0 and 1 can be **used at most once**.
* The given numbers of 0s and 1s will both not exceed 100
* The size of given string array won't exceed 600.

### Example
* Input: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
  * Output: 4
  * At most 4 strings can be formed by the using of 5 0s and 3 1s, which are “10,”0001”,”1”,”0”
* Input: Array = {"10", "0", "1"}, m = 1, n = 1
  * Output: 2
  * You could form "10", but then you'd have nothing left, and it's only one number. Better form "0" and "1", that's two numbers.

## Solution 1: 三维DP。用了“Offset One”的方法，即把“零”而非第一个元素作为某一个或几个维度的开始
这一题只要用 m个0 和 n个1 凑String数组里的strings，所以每个string里的0和1是怎么排列的根本不care，只care每个string里有几个0，几个1

这题明显用DP做。有点像 **背包问题里面 又限制总重量，又限制总体积 的题目**。可以用 三维DP 来做。
* dp[len + 1][m + 1][n + 1]，其中 dp[i][j][k] 表示 **用 j个0 和 k个1，最多能组成String数组里 前i个元素 中的几个**。注意这里是 前i个元素，不是元素index从0到i。这前i个元素里的任何元素都可能被或者不被组成
* 初始条件：dp[0][i][j] 总是 0。因为是要去组成数组里的第0个元素，而所谓的第0个元素根本不存在，所以永远都是只能组成 0个。既然是0，那么这个初始条件也就不用写出来了

注意
* m 和 n 都是可以等于 0 的

### Complexity
* Time: O(len * m * n), len是String数组的长度
* Space: O(len * m * n)

### Java
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0 || m < 0 || n < 0) {
            return 0;
        }
        
        int len = strs.length;
        int[][] counts = new int[len][2]; // number of 0s and 1s in each String
        
        // fill in the counts matrix
        for (int i = 0; i < len; i++) {
            for (char c : strs[i].toCharArray()) {
                if (c == '0') {
                    counts[i][0]++;
                } else { // c == '1'
                    counts[i][1]++;
                }
            }
        }
        
        int[][][] dp = new int[len + 1][m + 1][n + 1];
        
        for (int i = 1; i <= len; i++) {
            int zeroNum = counts[i - 1][0];
            int oneNum = counts[i - 1][1];
            
            for (int j = 0; j <= m; j++) { // j: number of zeroes，不要从0开始！有可能0个零
                for (int k = 0; k <= n; k++) { // k: number of ones，不要从0开始！有可能0个一
                    
                    dp[i][j][k] = dp[i - 1][j][k]; // 不使用 0 and/or 1 来凑第i个String的情况
                    
                    if (j >= zeroNum && k >= oneNum) { // 使用 0 and/or 1 来凑第i个String的情况
                        dp[i][j][k] = Math.max(dp[i][j][k], dp[i - 1][j - zeroNum][k - oneNum] + 1);
                    }
                }
            }
        }
        return dp[len][m][n];
    }
}
```

如果不用Offset One方法，则代码如下。可见会多一些行。但也没有复杂太多。不用Offset One也可能会使思路更清晰，有利有弊。
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0 || m < 0 || n < 0) {
            return 0;
        }
        
        int len = strs.length;
        int[][] counts = new int[len][2];
        
        // fill in the counts matrix
        for (int i = 0; i < len; i++) {
            for (char c : strs[i].toCharArray()) {
                if (c == '0') {
                    counts[i][0]++;
                } else { // c == '1'
                    counts[i][1]++;
                }
            }
        }
        
        int[][][] dp = new int[len][m + 1][n + 1]; // 改动
        
        // 初始化。如果使用Offset One方法，就不用写这些初始化了
        for (int i = counts[0][0]; i <= m; i++) {
            for (int j = counts[0][1]; j <= n; j++) {
                dp[0][i][j] = 1;
            }
        }
        
        for (int i = 1; i < len; i++) { // 改动
            int zeroNum = counts[i][0]; // 改动
            int oneNum = counts[i][1]; // 改动
            
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= n; k++) {
                    
                    dp[i][j][k] = dp[i - 1][j][k];
                    
                    if (j >= zeroNum && k >= oneNum) {
                        dp[i][j][k] = Math.max(
                            dp[i][j][k],
                            dp[i - 1][j - zeroNum][k - oneNum] + 1);
                    }
                }
            }
        }
        return dp[len - 1][m][n]; // 改动
    }
}
```

## Solution 2: 基于上面的三维DP方法，改进空间。时间无法再优化

### 空间改进（一旦空间降维，就要想到可能要从大到小填写DP矩阵！）
注意到，dp[i][#][#] 永远只用到 dp[i - 1][#][#]，所以只用到了前一“层”的dp，一共只用到“两层”dp，而不是len层的dp。len是给的String数组的长度。

再进一步想！这个所谓的两层，其实只用一层就够了！下一层不断地覆盖上一层的一部分，这叫做“循环数组”或者“滚动数组”（一维或二维或三维数组都可以是循环的）。详细看下面的代码

特别要注意的是！如果用了“降维打击”，去掉了DP矩阵的第一个维度，那么**原来三维DP的后两层的for循环都必须改为从大到小了**，即整体上从原来的“左上到右下”不得不变为“右下到左上”。原因是我们把分层去掉了，那么就只有这一层了，那么还按照老顺序从小到大的话，就会"踩着自己原来的脚印"，不断复制自己，造成结果高于实际值。
* 比如，如果第一个String有2个0和3个1，则在三层循环时，第一维进行第一次循环时，会得到 dp[2][3] = 1，然后 dp[4][6] = 2，然后 dp[6][9] = 3...... 那就越挫越离谱了

### Complexity
* Time: O(len * m * n), len是String数组的长度
* Space: O(m * n)

### Java
这个解法里也用了 Offset One 的方法
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        if (strs == null || strs.length == 0 || m < 0 || n < 0) {
            return 0;
        }
        
        int len = strs.length;
        int[][] counts = new int[len][2];

        for (int i = 0; i < len; i++) {
            for (char c : strs[i].toCharArray()) {
                if (c == '0') {
                    counts[i][0]++;
                } else { // c == '1'
                    counts[i][1]++;
                }
            }
        }
        
        int[][] dp = new int[m + 1][n + 1]; // 改动
        
        for (int i = 1; i <= len; i++) { // 虽然空间上能省去这一维，但时间上的这一维可不能少
            int zeroNum = counts[i - 1][0];
            int oneNum = counts[i - 1][1];
            
            // 下面的两层for循环都必须改为从大到小了，即整体上
            // 从原来的“左上到右下”不得不变为“右下到左上”
            // 原因是原来三维DP矩阵的第一维没了，现在只有第二三维构成的这么一“层”平面了
            for (int j = m; j >= 0; j--) { // 
                for (int k = n; k >= 0; k--) {
                    // 从dp[i][j][k] = dp[i - 1][j][k] 变成下面这个样子
                    // dp[j][k] = dp[j][k]; // 然后进一步发现这一句成了废话，可以不要了
                    
                    if (j >= zeroNum && k >= oneNum) {
                        dp[j][k] = Math.max(dp[j][k], dp[j - zeroNum][k - oneNum] + 1);
                    }
                }
            }
        }
        return dp[m][n];
    }
}
```

## Reference
* [Ones and Zeroes [LeetCode]](https://leetcode.com/problems/ones-and-zeroes/description/)

{% include links.html %}
