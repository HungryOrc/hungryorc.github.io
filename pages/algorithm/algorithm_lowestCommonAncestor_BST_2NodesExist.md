---
title: "Lowest Common Ancestor in BST for 2 Nodes that Exist"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_lowestCommonAncestor_BST_2NodesExist.html
folder: algorithm
toc: false
---

## Description
Given a binary search tree (BST), find the lowest common ancestor (LCA) of 2 given nodes in the BST. We guarantee that these 2
nodes do exist.

The lowest common ancestor is defined between two nodes v and w as the lowest node in the tree that has both v and w as descendants 
(where we allow a node to be the descendant of itself).
```
               6
           /        \
          2          8
        /   \       /  \
       0     4     7    9
            /  \
           3    5
```
For example, in the above tree, the lowest common ancestor of nodes 2 and 8 is 6. 
The LCA of nodes 2 and 5 is 2.
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

## Solution
哦也

### Complexity
* Time: O(tree height)
* Space: O(tree height)，call stack的层数

### Java
```java

```

## Reference
没找这题

{% include links.html %}
