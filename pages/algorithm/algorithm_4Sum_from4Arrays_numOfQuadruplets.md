---
title: "4 Sum from 4 Arrays, Return Number of Quadruplets whose sum equals to zero"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_4Sum_from4Arrays_numOfQuadruplets.html                               
folder: algorithm
toc: false
---

## Description
Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) 
there are such that A[i] + B[j] + C[k] + D[l] is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. 
All integers are in the range of -2^28 to 2^28 - 1 and the result is guaranteed to be at most 2^31 - 1.

Note:
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次
* **一个数组里的重复元素被当做不同的元素使用！比如，A里有两个1，第一个1和B,C,D里的数（各抽一个）可以组成k个我们要的quadruplets，
那么第二个1也可以和B,C,D里的数组成k个我们要的quadruplets，这加在一起就是 2k 个答案，它们之间互不影响！**

### Example
* Input: A = [ 1, 2], B = [-2,-1], C = [-1, 2], D = [ 0, 2]
  * Output: 2. The two tuples are:
    * (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
    * (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

## Solution：用一个map记录A和B的所有可能的和以及其次数，然后计算C和D的所有可能的和，同时看有多少A+B能和它们match
详见下面代码。很简明。这题并不需要事先排序这些数组

### Complexity
* Time: O(n^2)
* Space: O(n), map size

### Java
```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        // ignore validations
        
        // <sum, count>
        Map<Integer, Integer> mapAB = new HashMap<>();
        
        for (int a : A) {
            for (int b : B) {
                int sum = a + b;
                int count = mapAB.getOrDefault(sum, 0);
                mapAB.put(sum, count + 1);
            }
        }
        
        int numOfMatches = 0;
        for (int c : C) {
            for (int d : D) {
                int sum = c + d;
                
                // the target sum of all quadruplets is zero
                int countOfComplements = mapAB.getOrDefault(0 - sum, 0);
                numOfMatches += countOfComplements;
            }
        }
        return numOfMatches;
    }
}
```

## Reference
[4Sum II [LeetCode]](https://leetcode.com/problems/4sum-ii/description/)

{% include links.html %}
