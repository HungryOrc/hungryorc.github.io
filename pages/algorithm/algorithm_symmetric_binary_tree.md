---
title: "Check if a Binary Tree is Symmetric"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_symmetric_binary_tree.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
```java
public class TreeNode {
    public int key;
    public TreeNode left, right;
    public TreeNode(int key) {
        this.key = key;
    }
}
```

### Follow up
分别用recursion以及iteration做

### Example
略

## Solution 1: Recursion，前 1% 的速度

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree)，即call stack的层数

### Java
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        return isSym(root.left, root.right);
    }
    
    private boolean isSym(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } else if (left == null || right == null) {
            return false;
        }
        
        if (left.val != right.val) {
            return false;
        }
        
        return isSym(left.left, right.right) && isSym(left.right, right.left);
    }
}
```

## Solution 2: Iteration


### Complexity
* Time: O(
* Space: O(

### Java
```java

```

## Reference
* [Symmetric Tree [LeetCode]](https://leetcode.com/problems/symmetric-tree/description/)

{% include links.html %}
