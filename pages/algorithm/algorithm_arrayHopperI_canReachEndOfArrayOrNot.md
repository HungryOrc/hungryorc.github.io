---
title: "Array Hopper I: Can Reach the End of Array or Not"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_arrayHopperI_canReachEndOfArrayOrNot.html
folder: algorithm
toc: false
---

## Description
Given an array A of **non-negative** integers, you are initially positioned at index 0 of the array. 
A[i] means the maximum jump distance from that position (you can only jump towards the end of the array). 
Determine if you are able to reach the last index

Assumptions
* The given array is not null and has length of at least 1.
* If the length of the given array is 1, then we define the result to be true, even if array[0] = 0.
  * 数组最后一个元素一定可以达到最后一个元素，就算这个元素的值为0，它也达到了，因为它自己本身就是最后一个，不用走也自然达到

### Example
* Input: {1, 3, 2, 0, 3}
  * Output: true
* Input: {2, 1, 1, 0, 2}
  * Output: false
  
## Solution: DP

### Complexity
* Time: O(n)
* Space: O(n)

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
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
