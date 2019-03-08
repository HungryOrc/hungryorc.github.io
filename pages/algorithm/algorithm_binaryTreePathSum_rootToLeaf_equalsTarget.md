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
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [Path Sum [LeetCode]]()

{% include links.html %}
