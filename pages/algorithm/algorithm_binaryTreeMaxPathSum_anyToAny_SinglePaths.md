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
    public int maxPathSum(TreeNode root) {
        if (root == null) {
            return Integer.MIN_VALUE;
        }
    
        int[] maxPathSum = {Integer.MIN_VALUE};
        findMaxPathSum(root, root.val, 0, maxPathSum);
        return maxPathSum[0];
    }
  
    private void findMaxPathSum(TreeNode node, int curPrefixSum, int minPrefixSum, 
            int[] maxPathSum) {
        maxPathSum[0] = Math.max(maxPathSum[0], curPrefixSum - minPrefixSum);
        minPrefixSum = Math.min(curPrefixSum, minPrefixSum);
    
        if (node.left != null) {
            findMaxPathSum(node.left, curPrefixSum + node.left.val, minPrefixSum, maxPathSum);
        }
        if (node.right != null) {
            findMaxPathSum(node.right, curPrefixSum + node.right.val, minPrefixSum, maxPathSum);
        }  
    }
}
```

## Solution 2: 还是求每个path上的 max subarray sum. But 不用 prefix sum，而是用 DP 的思想做
DP数组的每一位，表示 **以当前位为结束点的最大的 max subarray sum**. 所以如果以前一位 为结束点的 max subarray sum < 0 的话，这一位就放弃前面的所有东西，从这一位开始，重新开始. 

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(tree height), call stack的层数 <=== 对么 ？？？

### Java
```java
public class Solution {
    public int maxPathSum(TreeNode root) {
        if (root == null) {
            return Integer.MIN_VALUE;
        }
    
        int[] maxPathSum = {Integer.MIN_VALUE};
        findMaxPathSum(root, 0, maxPathSum);
        return maxPathSum[0];
    }
  
    private void findMaxPathSum(TreeNode node, int maxSubarrayPathSum, int[] maxPathSum) {
        if (node == null) {
            return;
        }
    
        if (maxSubarrayPathSum < 0) {
            maxSubarrayPathSum = node.val;
        } else {
            maxSubarrayPathSum += node.val;
        }
    
        maxPathSum[0] = Math.max(maxPathSum[0], maxSubarrayPathSum);

        findMaxPathSum(node.left, maxSubarrayPathSum, maxPathSum);
        findMaxPathSum(node.right, maxSubarrayPathSum, maxPathSum);
    }
}
```

## Reference
Didn't go to find this question on the web yet

{% include links.html %}
