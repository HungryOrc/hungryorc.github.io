---
title: "Binary Tree Max Path Sum, from Any Node to Any Node, Single Paths"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTreeMaxPathSum_anyToAny_SinglePaths.html
folder: algorithm
toc: false
---

## Description
起始点和终止点，必须在一条**单路径**上。所谓的单路径，就是从整棵树的root到left的某一条路线上，
这样的路线可以左右拐弯，但不可以上下折返。

Given a binary tree in which each node contains an integer number. 
Find the maximum possible subpath sum(both the starting and ending node of the subpath 
should be on the same path from root to one of the leaf nodes, and the subpath is allowed to contain only one node).
Assumptions: The root of given binary tree is not null

Note: 
* Start node and end node **can be the same node**!
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
   -5
  /    \
2      11
     /    \
    6     14
           /
        -3
```
The maximum path sum is 11 + 14 = 25

### Example
See above

## Solution 1: Prefix Sum
对于每一个由root到leaf的path来说，就是求这个path上的 **Max Subarray Sum**！
所以可以用 `prefixSumFromRootToCurNode - minPrefixSumFromRootTillBeforeCurNode` 来做

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(tree height), call stack的层数 <=== 对么 ？？？

### Java
```java
public class Solution {
  int maxPathSum;
  
  public int maxPathSum(TreeNode root) {
    if (root == null) {
      return Integer.MIN_VALUE;
    }
    
    maxPathSum = Integer.MIN_VALUE;
    
    findMaxPathSum(root, root.key, 0);
    return maxPathSum;
  }
  
  private void findMaxPathSum(TreeNode node, int curPrefixSum, int minPrefixSum) {

    maxPathSum = Math.max(maxPathSum, curPrefixSum - minPrefixSum);
    
    minPrefixSum = Math.min(curPrefixSum, minPrefixSum);
    
    if (node.left != null) {
      findMaxPathSum(node.left, curPrefixSum + node.left.key, minPrefixSum);
    }
    if (node.right != null) {
      findMaxPathSum(node.right, curPrefixSum + node.right.key, minPrefixSum);
    }
  }
}
```

## Reference
Didn't go to find this question on the web yet

{% include links.html %}
