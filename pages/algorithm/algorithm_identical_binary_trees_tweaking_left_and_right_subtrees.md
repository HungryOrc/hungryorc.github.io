---
title: "Identical Binary Trees after Tweaking Left and Right Subtrees"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_identical_binary_trees_tweaking_left_and_right_subtrees.html
folder: algorithm
toc: false
---

## Description
如果两个树，能通过任意多次地tweak它们里面的任意nodes，实现两树的完全相同（所有位置所有值都相同），那么这两个树之间的关系就叫做 "tweaked identical"

Determine whether two given binary trees are identical assuming any number of ‘tweak’s are allowed. 
A "tweak" is defined as a swap of the children of **any** node in the tree.
```java
public class TreeNode {
    int key;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { key = x; }
}
```

### Example
* Input: Given the following 2 binary trees:
  ```
        5                 5
      /   \             /   \
     3     8           8     3
   /   \                   /   \
  1     4                 4     1
  ```
  * Output: they are "tweaked identical"

## Solution 1: Recursion

### Complexity
* Time: O(4^height of tree)
* Space: O(height of tree), call stack的层数

### Java
```java
public class Solution {
    public boolean isTweakedIdentical(TreeNode one, TreeNode two) {
        if (one == null && two == null) {
            return true;
        } else if (one == null || two == null) {
            return false;
        }
      
        // one != null && two != null
        if (one.key != two.key) {
            return false;
        }
      
        return (isTweakedIdentical(one.left, two.left) && isTweakedIdentical(one.right, two.right))
            || (isTweakedIdentical(one.left, two.right) && isTweakedIdentical(one.right, two.left));
    }
}
```



## Reference
* [Binary Tree Inorder Traversal [LeetCode]](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

{% include links.html %}
