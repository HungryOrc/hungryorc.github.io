---
title: "Validate Binary Search Tree"
tags: [algorithm, binary_search_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validate_bst.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:
1. The left subtree of a node contains only nodes with keys less than the node's key.
2. The right subtree of a node contains only nodes with keys greater than the node's key.
3. Both the left and right subtrees must also be binary search trees.
4. A single node tree is a BST.

Definition of TreeNode:
```java
public class TreeNode {
    public int val;
    public TreeNode left, right;
    public TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}
```

### Example
略
    
## Solution 1: Recursion



## Solution 2: Iteration
一个不含重复元素的BST，被in-order遍历的话，会形成一个单调上升的序列。
如此，我们就可用stack做一个中序遍历，把结果放到一个 array list 里，再验证是不是每个元素都比前一个大

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
```

## Reference

{% include links.html %}
