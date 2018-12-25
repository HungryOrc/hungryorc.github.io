---
title: "Paint House I: 3 Colors"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_paint_house_3_colors.html
folder: algorithm
toc: false
---

## Description
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note: All costs are positive integers.

### Example
* Input: [[17,2,17],[16,16,5],[14,3,19]]
  * Output: 10. Paint house 0 into blue, paint house 1 into green, paint house 2 into blue. Minimum cost: 2 + 5 + 3 = 10.

## Solution 1: DP
挺简明的逻辑。把3种颜色分开处理。dp[i][j] 表示：index 为 i 的房子涂了编号为j 的颜色 的前提下，从index 0 到 index i (inclusive at both ends) 的所有房子的总cost的最小值

### Complexity
* Time: O(n * m * (m - 1)) = O(n * m^2)，其中 n 是房子的个数，m 是颜色的个数
* Space: O(n * m)

### Java
```java
class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) {
            return 0;
        }
        
        int n = costs.length; // number of houses
        int m = 3; // number of colors

        int[][] dp = new int[n][m];
        
        // base case
        for (int i = 0; i < m; i++) {
            dp[0][i] = costs[0][i];
        }

        for (int i = 1; i < n; i++) { // for each house
            for (int j = 0; j < m; j++) { // for each color
                int minPrev = Integer.MAX_VALUE;
                
                for (int k = 0; k < m; k++) { // for each previous color
                    if (k == j) continue;
                    minPrev = Math.min(minPrev, dp[i - 1][k]); 
                }
                dp[i][j] = minPrev + costs[i][j];
            }
        }
        
        return Math.min(dp[n - 1][0], Math.min(dp[n - 1][1], dp[n - 1][2]));
    }
}
```

## Solution 1.1: 优化Solution1的空间，dp矩阵优化到 两行（似乎无法优化到一行），但会导致时间略微增加
Space O(n * m) 可以优化到 O(2 * m) 即 O(m)，因为每个dp[i][...] 只看 dp[i - 1][...]

这题不太能用滚动数组直接优化到一行dp数组，只好优化到两行的dp矩阵，因为dp矩阵在第一维一定以后，第二维之间的m个数是互相dependent的，所以不便于用滚动数组。用滚动数组的话，各个元素会互相叠加，导致错误

### Complexity
* Time: 和 Solution 1 一样，O(n * m^2)，其中 n 是房子的个数，m 是颜色的个数
* Space: O(m)

### Java
```java
class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) {
            return 0;
        }
        
        int n = costs.length; // number of houses
        int m = 3; // number of colors
        
        // 第一维从n变成2，
        // 其中dp[0][m] 代表 prev house，dp[1][m] 代表cur house
        int[][] dp = new int[2][m];
        
        // base case
        for (int i = 0; i < m; i++) {
            dp[0][i] = costs[0][i];
        }

        for (int i = 1; i < n; i++) { // for each house
            for (int j = 0; j < m; j++) { // for each color
                int minPrev = Integer.MAX_VALUE;
                
                for (int k = 0; k < m; k++) { // for each previous color
                    if (k == j) continue;
                    minPrev = Math.min(minPrev, dp[0][k]); 
                }

                dp[1][j] = minPrev + costs[i][j];
            }
            
            // 节约了空间，但多了下面这三步，导致多用了时间。虽然没造成时间上的量级增加
            for (int j = 0; j < m; j++) {
                dp[0][j] = dp[1][j];
            }
        }
        
        return Math.min(dp[0][0], Math.min(dp[0][1], dp[0][2]));
    }
}
```

## Solution 2: DP 优化时间，O(n * m^2) -> O(n * m)
Solution 1 的思路是，对于每个房子i的每个颜色j，看它前面那个房子(即i-1)除了j以外的颜色，哪一个cost最低。所以一共需要 n * m * m 的时间。
但是，其实我们每次比较j以外的颜色里的最低价的时候，都做了很多重复工作。特别是颜色如果很多的时候。
比如如果有100种颜色，那么除了颜色1以外，2-100的最小值，和除了2以外，1和3到100的最小值，里面是有大量的重复比较的，可以精简。
如果精简成功，那么 n * m * m 里面的最后一个m就可以没有，变成 n * m 的时间

精简的办法是：对于房子i-1的所有颜色，记录它的最低价 minCost，以及出现最低价格时的颜色 minCostColor，再记录第二低价 secondMinCost（第二低价时的颜色不用记了）。
当考察房子i的颜色j的时候（注意这里的意思是我们必须把房子i涂成颜色j了），如果 j 和 minCostColor 相同，那么我们就只能让房子i-1涂成第二低价的那种颜色了。如果 j 和 minCostColor 不同，那么我们自然可以让房子i-1涂成最低价的颜色。

### Complexity
* Time: O(n * m)，其中 n 是房子的个数，m 是颜色的个数
* Space: O(m)

### Java
代码看起来很长，其实逻辑不复杂。下面这样的做法做是为了之后更复杂的 k 个颜色的题目做准备的，本题只有3个颜色，这么做会显得小题大做
```java
public class Solution {
    public int minCost(int[][] costs) {
        if (costs == null || costs.length == 0) {
            return 0;
        }
        
        int n = costs.length; // number of houses
        int m = costs[0].length; // number of colors
        
        // dp[0][m] 代表 prev house，dp[1][m] 代表cur house
        int[][] dp = new int[2][m];
        
        // base case 变成了下面这几个参数
        Result result = getMinAndSecondMin(costs[0]);
        
        if (n == 1) {
            return result.min;
        }
        
        for (int i = 1; i < n; i++) { // for each house
            for (int j = 0; j < m; j++) { // for each color
                
                int prevMinCost = result.min;
                int prevMinColor = result.minIndex;
                int prevSecondMinCost = result.secondMin;
                
                if (j != prevMinColor) {
                    dp[1][j] = prevMinCost + costs[i][j];
                } else { // if(j == prevMiColor), 之前的 min color 就不能用了
                    dp[1][j] = prevSecondMinCost + costs[i][j];
                }
            }
            
            // 对于 house i 的各个颜色 j 都计算完以后，
            // 要把 house i 的各个参数算出来
            // 注意这里要用 dp[1]，不是用 costs[i] ！
            result = getMinAndSecondMin(dp[1]);
            
            for (int j = 0; j < m; j++) {
                dp[0][j] = dp[1][j];
            }
        }
        
        return getMinAndSecondMin(dp[0]).min;
    }
    
    // 要求输入的数组的长度 >= 2
    private Result getMinAndSecondMin(int[] array) {
        int min = array[0];
        int minIndex = 0;
        int secondMin = Integer.MAX_VALUE;
        
        for (int i = 1; i < array.length; i++) {
            int cur = array[i];
            if (cur <= min) {
                secondMin = min;
                min = cur;
                minIndex = i;
            } else if (cur < secondMin) { // && cur > min
                secondMin = cur;
            }
        }
        
        return new Result(min, minIndex, secondMin);
    }
}

// A helper class, 让参数传递更明晰。不是必须的
class Result {
    int min, minIndex, secondMin;
    public Result(int m, int mi, int sm) {
        this.min = m;
        this.minIndex = mi;
        this.secondMin = sm;
    }
}
```

## Reference
* [Paint House [LeetCode]](https://leetcode.com/problems/paint-house/description/)

{% include links.html %}
