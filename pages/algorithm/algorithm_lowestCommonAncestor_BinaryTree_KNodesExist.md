---
title: "Lowest Common Ancestor in Binary Tree for K Nodes that Exist"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_lowestCommonAncestor_BinaryTree_KNodesExist.html
folder: algorithm
toc: false
---

## Description
Given K nodes in a binary tree, find their lowest common ancestor.
Assumptions: K >= 2

There is no parent pointer for the nodes in the binary tree
The given K nodes are guaranteed to be in the binary tree
```
        5
      /   \
     9     12
   /  \      \
  2    3      14
```
```java
LCA(2, 3, 14) = 5
LCA(2, 3, 9) = 9
```
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    public TreeNode(int x) { val = x; }
}
```

### Example
见上文

## Solution：Recursion
因为这k个node都一定存在，所以我们可以用下面的方法：在从下到上的搜寻过程中，在最靠上的层级处看到谁，就返回谁！

### Complexity
* Time: O(tree height)
* Space: O(tree height)，call stack的层数

### Java
```java
public class Solution {
  
  public TreeNode lowestCommonAncestor(TreeNode root, List<TreeNode> nodes) {
    if (root == null) {
      return null;
    }
    if (nodes.contains(root)) {
      return root;
    }
    
    TreeNode outcomeFromLeftSubtree = lowestCommonAncestor(root.left, nodes);
    TreeNode outcomeFromRightSubtree = lowestCommonAncestor(root.right, nodes);
    
    if (outcomeFromLeftSubtree != null && outcomeFromRightSubtree != null) {
      return root;
    }
    if (outcomeFromLeftSubtree != null) { // && outcomeFromRightSubtree == null
      return outcomeFromLeftSubtree;
    } else { // outcomeFromLeftSubtree == null) && outcomeFromRightSubtree != null
      return outcomeFromRightSubtree;
    }
  }
}
```

## Reference
没找这题

{% include links.html %}
