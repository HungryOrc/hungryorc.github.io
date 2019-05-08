---
title: "Binary Tree Max Path Sum, from Any Node to Any Node, U Turn Paths"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTreeMaxPathSum_anyToAny_UTurnPaths.html
folder: algorithm
toc: false
---

## Description
Given a binary tree (不是 BST), in which each node contains an integer number.
Find the maximum possible sum from any node to any node. 本题为LC hard 难度。

Note: 
* The nodes' values can be **NEGATIVE integers**
* Start node and end node **can be the SAME node**
* The root of the given binary tree is not null

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```
For example, given the below binary tree:
```
   -1
  /    \
2      11
     /    \
    6    -14
```
* One example of paths could be -14 -> 11 -> -1 -> 2
* Another example could be the node 11 itself
The maximum path sum in the above binary tree is 6 + 11 + (-1) + 2 = 18

### Example
略

## Solution: DFS，精华在于：对于每个subtree的root，它要计算的东西，和它要往再上一层传递的东西，是不同的!
* 本层要计算和更新的，是 **“人字形”** 的双路径之和
* 本层要往上一层传递的，是 **“一条线”** 的单路径之和

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(tree height), call stack的层数 <=== 对么 ？？？

### Java
```java
public class Solution {
    public int maxPathSum(TreeNode root) {
        if (root == null) {
            return 0;
        }
    
        // 长度为1的数组，目的是方便进行reference的传递
        // 这里不能设为0！因为可能整棵树都是负数！那么最后结果一定是负数！
        int[] maxPathSum = { Integer.MIN_VALUE };
        dfs(root, maxPathSum);
        return maxPathSum[0];
    }
  
    private int dfs(TreeNode node, int[] maxPathSum) {
        if (node == null) {
            return 0;
        }
    
        // 要是小于0的话，还不如不加上它
        int maxSinglePathSumL = Math.max(0, dfs(node.left, maxPathSum));
        int maxSinglePathSumR = Math.max(0, dfs(node.right, maxPathSum));
    
        // 精华1！更新的是“人字形”的双路径之和，和下面的返回值不同
        maxPathSum[0] = Math.max(maxPathSum[0],
            node.val + maxSinglePathSumL + maxSinglePathSumR);
    
        // 精华2！返回的是“一条线”的单路径之和，和上面的更新值不同
        return node.val + Math.max(maxSinglePathSumL, maxSinglePathSumR);
    }
}
```

## Reference
* [Binary Tree Maximum Path Sum [LeetCode]](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)

{% include links.html %}
