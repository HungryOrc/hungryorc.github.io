---
title: "Lowest Common Ancestor in Binary Tree for 2 Nodes that Exist"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_lowestCommonAncestor_BinaryTree_2NodesExist.html
folder: algorithm
toc: false
---

## Description
Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

Notice: Assume these 2 nodes do exist in this tree.
```
  4
 / \
3   7
   / \
  5   6
```
```java
LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7
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

## Solution 1：Recursion

### Complexity
* Time: O(tree height)
* Space: O(tree height)，call stack的层数

### Java
```java
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {        
        if ((p.val <= root.val && q.val >= root.val) || 
            (p.val >= root.val && q.val <= root.val) {
            return root;
        }
        else if (p.val >= root.val && q.val >= root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        else { // if (p.val <= root.val && q.val <= root.val)
            return lowestCommonAncestor(root.left, p, q);
        }
    }
}
```

## Reference
没找这题

{% include links.html %}
