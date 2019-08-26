---
title: "Longest Univalue Path"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longestPath_tree_univalue.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.
The length of path between two nodes is represented by the number of edges between them.
```java
 public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```
Note: The given binary tree has not more than 10000 nodes. The height of the tree is not more than 1000.

注意！一字型的路径要算，A字型即拐弯的路径也要算！

### Example
* Input: 
  ```
              5
             / \
            4   5
           / \   \
          1   1   5
  ```
  * Output: 2
* Input: 
  ```
              1
             / \
            4   5
           / \   \
          4   4   5
  ```
  * Output: 2, the 4->4->4

## Solution: 我的实现，速度前1%，关键在于：每一个node往上一层传递的信息，和在本一层更新到glocal variable 里的信息，是两类东西！

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public class Solution {
    public int longestUnivaluePath(TreeNode root) {
        if (root == null) return 0;
        int[] maxLen = new int[]{1};
        
        dfs(root, maxLen);
        return maxLen[0] - 1;
    }
    
    private int dfs(TreeNode node, int[] maxLen) {
        if (node == null) return 0; // 显然这种情况下不需要更新maxLen了
        
        int leftOutcome = 0;
        if (node.left != null) {
            if (node.left.val == node.val) {
                leftOutcome = dfs(node.left, maxLen);
            } else {
                dfs(node.left, maxLen);
            }
        }
        
        int rightOutcome = 0;
        if (node.right != null) {
            if (node.right.val == node.val) {
                rightOutcome = dfs(node.right, maxLen);
            } else {
                dfs(node.right, maxLen);
            }
        }
        
        // 这里更新的是 以当前node为顶点的一字型或A字型的 univalue路径，最长有多长
        maxLen[0] = Math.max(maxLen[0], 1 + leftOutcome + rightOutcome);
        
        // 这里返回的是 以当前node为顶点的一字型的 univalue路径，最长有多长
        return Math.max(1 + leftOutcome, 1 + rightOutcome);
    }
}
```

## Reference
* [Longest Univalue Path [LeetCode]](https://leetcode.com/problems/longest-univalue-path/description/)

{% include links.html %}
