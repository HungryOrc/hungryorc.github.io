---
title: "Binary Tree Preorder Traversal"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_preorder.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, return the preorder traversal of its nodes' values.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

先root，再左子树，再右子树

### Example
* Input: Given binary tree {1,#,2,3}:
  ```
   1
    \
     2
    /
   3
  ```
  * Output: [1, 2, 3]

## Solution 1: Iteration with a Stack


### Complexity
* Time: O(n), size of the stack
* Space: O(n)

### Java
```java

```

## Reference
* [Binary Tree Preorder Traversal [LeetCode]](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

{% include links.html %}
