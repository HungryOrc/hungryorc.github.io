---
title: "Array Hopper III: Min Steps to Jump Out of the Array"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_arrayHopper3_minStepsToJumpOutOfArray.html
folder: algorithm
toc: false
---

## Description
Given an array of non-negative integers, you are initially positioned at index 0 of the array. 
A[i] means the maximum jump distance from that position (you can only jump towards the end of the array). 
Determine the minimum number of jumps you need to **jump out of the array**.

By jump out, it means you can not stay at the end of the array. Return -1 if you can not do so.

Assumptions
* The given array is not null and has length of at least 1.
* 数组最后一个元素如果是0，那么从它出发，或者任何jump到了它，都是无法继续走下去的，也就是不能 跳出这个数组的

### Example
* Input: {1, 3, 2, 0, 2}
  * Output: 3, from index 0 jump to index 1 then to the end of array, then jump out
* Input: {3, 2, 1, 1, 0}
  * Output: -1, no way to jump out of the array
  
## Solution: DP
从后往前搞。dp[i] 表示从元素i 出发，跳出数组所需的最少步数。注意最后一个元素未必能跳出数组，如果它的值是0就跳不出去。然后一个一个往前检查。具体逻辑见代码

### Complexity
* Time: O(n^2)，2层 for loop
* Space: O(n), dp array. This question CANNOT be simplified to a O(1) space complexity.

### Java
```java
public class Solution {
    public int minJump(int[] jumps) {
        if (jumps == null || jumps.length == 0) {
            return -1;
        }
      
        int n = jumps.length;
        int[] dp = new int[n];
        
        // base case
        if (jumps[n - 1] == 0) {
            dp[n - 1] = Integer.MAX_VALUE; // 从n-1处 不可能跳出去
        } else {
            dp[n - 1] = 1; // 一步就能跳出去
        }
      
        for (int i = n - 2; i >= 0; i--) {
            int radius = jumps[i];
          
            if (i + radius >= n) {
                dp[i] = 1;
                continue;
            }
          
            dp[i] = Integer.MAX_VALUE;
            for (int j = i + radius; j >= i + 1; j--) {
                if (dp[j] == Integer.MAX_VALUE) { // 别忘了这个！不可能的j就不要了！别用它加1
                    continue;
                }
              
                dp[i] = Math.min(dp[i], dp[j] + 1);
            }
        }
      
        if (dp[0] == Integer.MAX_VALUE) {
            return -1;
        }
        return dp[0];
    }
}
```

## Reference
* [Array Hopper III [LaiCode]](https://app.laicode.io/app/problem/90)

{% include links.html %}
