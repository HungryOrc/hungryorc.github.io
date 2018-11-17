---
title: "Burst Balloons"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_burst_balloons.html
folder: algorithm
toc: false
---

## Description
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. 
You are asked to burst all the balloons. If the you burst balloon i you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. 
After the burst, the i - 1 and i + 1 balloons becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

* You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them. 所以如果打到 index = 0 或者 
n - 1 的气球，就用这个规则计分
* 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100。这里 500 这个数字其实就是一个暗示！暗示要用 DP 来做。因为 500 量级用DFS做的话过于巨大，会冒

### Example
* Input: [3,1,5,8]
  * Output: 16
  ```
   nums  =  [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  -->  []
            burst 1       burst 5      burst 3     burst 8     all balloons gone
  coins  =   3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   =  167
  ```

## Solution: 中心开花 DP
这一题的难点在于，如何处理爆掉某一个气球以后，它两边的气球变得相邻。那么原来相距好远的气球也可能相邻。这样的不断缩并，用DP做是较好的方法。

用 **中心开花** 的DP方法来做。
* 最后所有的气球都是要爆的。我们可以从编号为i到j的一个区间的所有气球被爆掉这样的案例入手
* 在i到j这一段上，最后一枪打爆的可能是从i到j的任何一个气球，i和j都是inclusive。那么从i到j全部被打爆的最高的总得分，就是这j-1+1种最后一枪的情况里，
得分最高的那一个。
  * 这最后一枪不论打在哪里，它都会把整段分出左右两个分段，那么在分段上也是同样的方法论。如果最后一枪打在i或j，代码处理起来也是一样的。见下面代码

步骤：
* State: dp[i][j] 意思是从 index 为 i - 1 到 j - 1 的气球，把它们都打爆，能得到的最多的总得分是多少
* Init: dp[i][i] = nums[i - 1]，注意dp里的 i 对应到气球是 i - 1。dp[i][i] 一般来说最终都会大于 nums[i - 1]，因为还要乘以左右的数，但用 nums[i - 1] 作为它的初始值，以后会更大，也ok
* Induction rule: 如果对于 index 为 i - 1 到 j - 1 的所有气球，最后一个被打爆的是 index 为 k - 1 的那个气球，那么有：
  ```java
  // i <= k <= j
  dp[i][j] = max(dp[i][k - 1] + dp[k + 1][j] + nums[k - 1] * nums[i - 2] * nums[j])
  ```
  * 其中 nums[i - 2] 代表了 气球 i - 1 左边的那个气球，nums[j] 代表了 气球 j - 1 右边的那个气球
  * 这个递推公式的理由是：这一段上的其他气球已经都被打爆了，现在只剩下一个气球，那么最后打爆它的时候，它的分数只能和这个区间段外面相邻的2个气球的分数相乘了
* 结果：dp[1][n]，代表了打爆 index = 0 到 index = n - 1 的所有气球的 最高可能得分

### Complexity
* Time: O(n^3)
* Space: O(n^2), dp矩阵

### Java
```java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        
        // 气球有n个，用index 1 到 n 装这n个气球，
        // 用index 0 装第一个气球的左边，用index n+1 装第n个气球的右边
        int[] newNums = new int[n + 2];
        newNums[0] = 1;
        newNums[n + 1] = 1;
        for (int i = 1; i <= n; i++) {
            newNums[i] = nums[i - 1];
        }
        
        int[][] dp = new int[n + 2][n + 2];
        
        for (int len = 1; len <= n; len++) {
            
            // 从第i个气球到第j个气球，注意所有气球的index都必须是
            // 从 1 到 n 之间
            for (int i = 1; i + len - 1 <= n; i++) {
                int j = i + len - 1;
                
                for (int k = i; k <= j; k++) {
                    dp[i][j] = Math.max(dp[i][j],
                        dp[i][k - 1] + dp[k + 1][j] + 
                        newNums[k] * newNums[i - 1] * newNums[j + 1]);
                }
            }
        }
        return dp[1][n];
    }
}
```

## Reference
* [Burst Balloons [LeetCode]](https://leetcode.com/problems/burst-balloons/description/)

{% include links.html %}
