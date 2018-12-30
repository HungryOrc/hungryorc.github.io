---
title: "3 Sum, Return All Unique Triplets (the Order of the 3 Numbers in a Triplet Doesn't Matter)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_3Sum_numOfUniqueIndexTriplets.html                               
folder: algorithm
toc: false
---

## Description
注意这题 **要返回的是 unique triplets of indexes**，不是 triplets of element values。而 indexes 是永远不会重复的，所以 triplets of indexes 也永远不会重复，不管 element values 是否有重复。
* 除非故意把 index triplet 的顺序打乱，比如 indexes [1,2,3] 和 [1,3,2] 是重复的，但我们按照index从小到大的顺序来做，也不会出现这种情况

Given an integer array A, and an integer target, return the number of tuples i, j, k  such that i < j < k and A[i] + A[j] + A[k] == target.
* 3 <= A.length <= 3000
* **0 <= A[i] <= 100**
* **0 <= target <= 300**
* **最后这两个条件特别重要！即数组里每个元素都 >= 0，且 target >= 0，如果没这两个条件，用 DP 做就要费点事：就需要把所有数平移，以便把负数纳入到DP矩阵里去，因为矩阵里各个维度的序号自然都必须是非负数**

As the answer can be very large, return it modulo 10^9 + 7. 这是这一题的特殊要求，和主干逻辑无关，其他类似的题也不需要做这个

Note:
* The solution set must not contain duplicate triplets of indexes.
* [1,1,2] and [2,1,1] and [1,2,1] (all the numbers here are indexes) are considered as the same triplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: A = [1,1,2,2,3,3,4,4,5,5], target = 8
  * Output: 20
  * Enumerating by the values (A[i], A[j], A[k]):
    ```
    (1, 2, 5) occurs 8 times;
    (1, 3, 4) occurs 8 times;
    (2, 2, 4) occurs 2 times;
    (2, 3, 3) occurs 2 times.
    ```
* A = [1,1,2,2,2,2], target = 5
  * Output: 12
  * A[i] = 1, A[j] = A[k] = 2 occurs 12 times:
    ```
    We choose one 1 from [1,1] in 2 ways,
    and two 2s from [2,2,2,2] in 6 ways.
    ```

## Solution：DP，我自己的想法，感觉思路是比较给力的，但实测速度很慢，还没看leetcode的答案

### Complexity
* Time: O(n * 3 * target) = O(n * target)，其中 n 是数组里的元素个数，3是3Sum所要求的三个元素的意思
* Space: O(n * 3 * target) = O(n * target)

### Java
```java
class Solution {
    private static final int MOD = (int)(Math.pow(10, 9) + 7);
     
    public int threeSumMulti(int[] nums, int target) {
        if (nums == null || nums.length < 3) {
            return 0;
        }
        
        int n = nums.length;
        int[][][] dp = new int[n][3 + 1][target + 1];
        
        for (int i = 0; i < n; i++) {
            if (nums[i] <= target) {
                for (int j = i; j < n; j++) {
                    dp[j][1][nums[i]]++;
                }
            }
        }
        
        for (int j = 2; j <= 3; j++) {
            for (int i = j - 1; i < n; i++) {
                for (int k = 0; k <= target; k++) {
                    
                    dp[i][j][k] = dp[i - 1][j][k];
                    
                    if (nums[i] <= k) {
                        dp[i][j][k] += dp[i - 1][j - 1][k - nums[i]];
                        dp[i][j][k] %= MOD;
                    }
                }
            }
        }
        return dp[n - 1][3][target];
    }
}
```

## Reference
[3Sum With Multiplicity [LeetCode]](https://leetcode.com/problems/3sum-with-multiplicity/description/)

{% include links.html %}
