---
title: "Paint House II: K Colors"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_paint_house_k_colors.html
folder: algorithm
toc: false
---

## Description
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x k` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note: All costs are positive integers.

### Follow up
Could you solve it in O(nk) runtime? 这题的难度是 hard。

### Example
* Input: [[1,5,3],[2,9,4]]
  * Output: 5. Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5. Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5. 

## Solution: DP
Paint House 这类题的 DP 做法有一个进化的过程。具体的进化过程见我归纳的 [Paint House: 3 Colors]() 那一题，这里就不写了。
这一题 K Colors 和 3 Colors 在本质上是一样的，而且事实上 K Colors 我们这里用的代码 和 3 Colors 那题进化到最高级形式（Solution 2）时的代码
是**一模一样**的。

### Complexity
* Time: O(n * m) = O(n * m)，其中 n 是房子的个数，m 是颜色的个数
* Space: O(n * m)

### Java
```java
public class Solution {
    public int minCostII(int[][] costs) {
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
* [Paint House II [LeetCode]](https://leetcode.com/problems/paint-house-ii/description/)

{% include links.html %}
