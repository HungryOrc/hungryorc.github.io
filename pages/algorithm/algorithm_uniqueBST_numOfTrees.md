---
title: "Number of Unique Binary Search Trees"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_uniqueBST_numOfTrees.html
folder: algorithm
toc: false
---

## Description
Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

### Example
* Input: 3
  * Output: 5
  ```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
  ```

## Solution：我的DP方法

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    public int numTrees(int n) {
        if (n <= 0) {
            return 0;
        }
        
        int[] dp = new int[n + 1];
        if (n >= 1) {
            dp[0] = 1; // 这里是要参与左右子树的乘法的，所以dp[0]=1但是同时n=0则return 0
            dp[1] = 1;
        }
        if (n >= 2) {
            dp[2] = 2;
        }
        
        for (int i = 3; i <= n; i++) {
            for (int j = 0; j <= i - 1; j++) {
                dp[i] += dp[j] * dp[i - 1 - j];
            }
        }
        return dp[n];
    }
}
```

## Reference
* [Unique Binary Search Trees [LeetCode]](https://leetcode.com/problems/unique-binary-search-trees/description/)

{% include links.html %}
