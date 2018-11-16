---
title: "Check if a Tree is a Binary Search Tree"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validate_binary_search_tree.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

```java
public class TreeNode {
    int key;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
略

## Solution: Recursion，速度 前1%

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree), call stack的层数

### Java
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }        
        
        // 按本题题意，node的val可以取到int数据类型的上下限值，
        // 所以这里不得不用long型的上下限作为不可达到的边界
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean validate(TreeNode node, long min, long max) {
        if (node == null) {
            return true;
        }
        // 按本题题意，BST里面不可以存在相同val的nodes，
        // 所以一个node的val如果和它的parent node的val相同，也要返回false
        if (node.val >= max || node.val <= min) {
            return false;
        }
        
        return validate(node.left, min, node.val) && 
               validate(node.right, node.val, max);
    }
}
```

## Reference
* [Validate Binary Search Tree [LeetCode]](https://leetcode.com/problems/validate-binary-search-tree/description/)

{% include links.html %}
