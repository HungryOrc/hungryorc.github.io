---
title: "Array Hopper II: Min Steps to Reach the End of Array"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_arrayHopper2_minStepsToReachEndOfArray.html
folder: algorithm
toc: false
---

## Description
Given an array A of non-negative integers, you are initially positioned at index 0 of the array. A[i] means the maximum jump distance from index i (you can only jump towards the end of the array). Determine the minimum number of jumps you need to reach the end of array. 

If you can not reach the end of the array, return -1.

Assumptions
* The given array is not null and has length of at least 1.
* If the length of the given array is 1, then we define the result to be 0, even if array[0] = 0.
  * 数组最后一个元素一定可以达到最后一个元素，就算这个元素的值为0，它也达到了，因为它自己本身就是最后一个，不用走也自然达到

### Example
* Input: {1, 3, 2, 0, 3}
  * Output: 2
* Input: {2, 1, 1, 0, 2}
  * Output: -1
  
## Solution: DP
从后往前搞。dp[i] 表示从元素i 出发，能否到达最后一个元素。最后一个元素一定能达到最后一个元素，然后一个一个往前检查

对于元素i，如果在它的向后的max hop distance以内的任何元素可以到达最后的元素，则i也可以到达最后的元素，此时此次loop检查就可以终止，
然后开始对元素 i-1 的检查

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
* [Array Hopper II [LaiCode]](https://app.laicode.io/app/problem/89)

{% include links.html %}
