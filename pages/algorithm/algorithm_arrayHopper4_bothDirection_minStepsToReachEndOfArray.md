---
title: "Array Hopper IV: Can Go Both Directios, Return Min Steps to Reach the End of Array"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_arrayHopper4_bothDirection_minStepsToReachEndOfArray.html
folder: algorithm
toc: false
---

## Description
Given an array A of non-negative integers, you are initially positioned at an arbitrary index of the array. 
A[i] means the maximum jump distance from that position (you can **either jump left or right**). 
Determine the minimum jumps you need to reach the right end of the array. 

Return -1 if you can not reach the right end of the array.

Assumptions
* The given array is not null and has length of at least 1.
* If the length of the given array is 1, then we define the result to be 0, even if array[0] = 0.
  * 数组最后一个元素一定可以达到最后一个元素，就算这个元素的值为0，它也达到了，因为它自己本身就是最后一个，不用走也自然达到

### Example
* Input: {1, 3, 1, 2, 2}, initial index = 2
  * Output: 2. Jump to index 1 then to the right end of array
* Input: {2, 4, 1, 0, 0, 0}, initial index = 2
  * Output: 3. Jump to index 1 then to the right end of array
* Input: {4, 0, 1, 0, 0}, initial index = 2
  * Output: -1. Cannot reach the end of array starting at initial index = 2, no matter how you go left/right with any number of steps

## Solution: 一种综合了DP和图遍历的思想。其实DP就是一种图遍历。对于这题来说，相当于一种“图上的最短路径”
这题的
## Solution: 一种综合了DP和图遍历的思想。其实DP就是一种图遍历。对于这题来说，相当于一种“图上的最短路径
从后往前搞。dp[i] 表示从元素i 出发，到达最后一个元素的最少步数。最后一个元素一定能达到最后一个元素，然后一个一个往前检查。具体逻辑见代码

### Complexity
* Time: O(n^2)，2层 for loop
* Space: O(n). This question CANNOT be simplified to a O(1) space complexity.

### Java
```java
public class Solution {
    public boolean canJump(int[] jumps) {
        if (jumps == null || jumps.length == 0) {
            return false;
        }
      
        int n = jumps.length;
        boolean[] dp = new boolean[n];
        dp[n - 1] = true;
      
        for (int i = n - 2; i >= 0; i--) {
            int curMaxJump = jumps[i];
            int maxDestination = i + curMaxJump;
            
            if (maxDestination >= n - 1) {
                dp[i] = true;
                continue; // 可以到下一个i去了
            }
            
            for (int j = maxDestination; j >= i + 1; j--) {
                if (dp[j]) {
                    dp[i] = true;
                    break; // 可以到下一个i去了
                }
            }
        }
        return dp[0];
    }
}
```

## Reference
* [Array Hopper IV [LaiCode]](https://app.laicode.io/app/problem/91)

{% include links.html %}
