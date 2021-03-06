---
title: "Assign + and - to Integers to Meet the Target Sum"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_assignPlusAndMinus_toNumbers_toMeetTargetSum.html
folder: algorithm
toc: false
---

## Description
You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. 
For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

* The length of the given array is positive and will not exceed 20.
* The sum of elements in the given array will not exceed 1000. 
  * 这个数1000就是在暗示，尽量别用DFS，时间消耗太大。应该用 DP
* Your output answer is guaranteed to be fitted in a 32-bit integer.

### Clarification Questions
这一题应该向面试官问这些问题：
* 是不是必须给所有数assign正负号？能否对某些数什么都不做？
* 一开始那些数有没有负数？有的话它们能否标负号？
* 这些数全部加在一起是多少？为了看能否设dp矩阵做准备！
  * 不要一开始自己觉得加起来的和可能太大，就自己否定了！说不定玄机就在这里面
* target有没有可能大于所有数的绝对值加在一起的和？

### Example
* Input: [1, 1, 1, 1, 1], target is 3.
  * Output: 5, the five ways to meet 3 are:
    ```
    -1+1+1+1+1 = 3
    +1-1+1+1+1 = 3
    +1+1-1+1+1 = 3
    +1+1+1-1+1 = 3
    +1+1+1+1-1 = 3
    ```

## Solution 1：DP
这题一看就是DP的感觉。但是难点在于，induction rule不好写，
因为`dp[i][target]`和`dp[i - 1][target]`没有直接关系，
`dp[i][target]`是和`dp[i - 1][target - nums[i]]`以及`dp[i - 1][target + nums[i]]`有直接关系

突破口在于：题目给了个hint，说所有数字加在一起不会超过1000，我们可以认为如果把所有数都取abs，再加起来，也不会很大。
这样就可以用DP来做了：**把各种sum都 + 1000**

// definition
// dp[i][1000 + j]
// how many ways to assign symbols from a sublist from 0 and ending at i-th element of non-negative integers, a1, a2, ..., an, to make sum of integers equal to target j. For each integer, you should choose one from + and - as its new symbol.

// base case
// 
// dp[0][1000 + nums[0]] = 1
// dp[0][1000 -nums[0]] = 1
// else 
// dp[0][x] = 0

// dp[i][j]     nums[i]
// case1: dp[i - 1][1000 + j - nums[i]]   : +
// case2: dp[i - 1][1000 + j + nums[i]]  :  -
// dp[i][1000 + j] = case1 + case2

// result
// dp[n - 1][1000 + S]


下面这种做法比较反直觉，不要采用，只做备案：


如下
* State: dp[i][j]：用index从0到i的num，任意加减组成j，共有多少种方式
  * int[][] dp = new int[n][maxSum * 2 + 1]，其中 maxSum 是数组nums里的所有数都取绝对值以后的和
  * 第二维要用 maxSum * 2 + 1，是为了能表达负数，即从 -maxSum 到 0 到 maxSum 的所有可能值，这些值都是数组里的数有可能组合成的
    * 举例，如果数组里的数加加减减之后的和为0，则第二维的坐标应该是 0 + maxSum = maxSum

### Complexity
* Time: O(n * maxSum)
* Space: O(n * maxSum), dp矩阵

### Java
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int n = nums.length;
        int maxSum = 0;
        for (int i = 0; i < n; i++) {
            nums[i] = Math.abs(nums[i]); // 为了之后的方便
            maxSum += nums[i];
        }
        
        // 小心这个，不然最后 return 结果的时候，第二维会越界
        if (Math.abs(target) > maxSum) {
            return 0;
        }
        
        int[][] dp = new int[n][maxSum * 2 + 1];
        
        // init
        // 这里用 += 1 而非 = 1，是为了规避一个很恶心的陷阱：
        // 当nums[0]为0的时候，如果写 = 1，那么dp[0][0] 最后就是 1，但其实
        // dp[0][0] 最后应该是 2，因为 +0 和 -0 是两种方式！
        // 后面 i >= 1 的情况就不用管这个了，因为后面都用的是 += dp[...][...]
        dp[0][nums[0] + maxSum] += 1;
        dp[0][-1 * nums[0] + maxSum] += 1;
        
        for (int i = 1; i < n; i++) {
            int cur = nums[i];
            
            for (int sum = -1 * maxSum; sum <= maxSum; sum++) {
                if (sum - cur >= -1 * maxSum) {
                    dp[i][sum + maxSum] += dp[i - 1][sum - cur + maxSum];
                }
                if (sum + cur <= maxSum) {
                    dp[i][sum + maxSum] += dp[i - 1][sum + cur + maxSum];
                }
            }
        }
        return dp[n - 1][target + maxSum];
    }
}
```

## Solution 2：DFS，速度要慢很多，竟然也通过了leetcode
可以看一下写法

### Complexity
* Time: O(n^2)
* Space: O(n), DFS的call stack有n层

### Java
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int[] count = new int[1]; // value is 0
        dfs(nums, 0, 0, target, count);
        return count[0];
    }
    
    private void dfs(int[] nums, int index, int curSum, 
                     int target, int[] count) {
        if (index == nums.length) {
            if (curSum == target) {
                count[0]++;
            }
            return;
        }
        
        dfs(nums, index + 1, curSum + nums[index], target, count);
        dfs(nums, index + 1, curSum - nums[index], target, count);
    }
}
```

## Reference
* [Target Sum [LeetCode]](https://leetcode.com/problems/target-sum/description/)

{% include links.html %}
