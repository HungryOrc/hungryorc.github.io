---
title: "Binary Tree Path Sum, from Root to Leaf, Equals to Target"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTreePathSum_rootToLeaf_equalsTarget.html
folder: algorithm
toc: false
---

## Description
Given a binary tree (不是 BST) and a sum, 
determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```
For example, given the below binary tree and sum = 22:
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

### Example
略

## Solution 1: Recursion

### Complexity
* Time: O(n)
* Space: O(tree height), call stack的层数

### Java
```java
public class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        
        // 到当前节点为止，正好加和为sum。而且当前节点正好是一个leaf
        if (root.val == sum && root.left == null && root.right == null) {
            return true;
        }

        sum -= root.val;
        return (hasPathSum(root.left, sum) || hasPathSum(root.right, sum));
    }
}
```

## Solution 2: Iteration DFS，2个Stack。如果用2个Queue做BFS，也是大同小异

### Complexity
* Time: O(n)
* Space: O(n)，stack size

### Java
```java
public class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        
        Deque<TreeNode> nodeStack = new ArrayDeque<>();
        nodeStack.push(root);
        Deque<Integer> sumStack = new ArrayDeque<>();
        sumStack.push(root.val);
        
        while(!nodeStack.isEmpty()) {
            TreeNode curNode = nodeStack.pop();
            int curSum = sumStack.pop();
            
            if (curSum == sum && curNode.left == null && curNode.right == null) {
                return true;
            }
            
            if (curNode.left != null) {
                nodeStack.push(curNode.left);
                sumStack.push(curSum + curNode.left.val);
            }
            if (curNode.right != null) {
                nodeStack.push(curNode.right);
                sumStack.push(curSum + curNode.right.val);
            }
        }
        return false;
    }
}
```

## Reference
* [Path Sum [LeetCode]]()

{% include links.html %}
