---
title: "Max (Visible) Nodes along Each Path in a Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_max_visible_nodes_along_each_path_in_binary_tree.html
folder: algorithm
toc: false
---

## Description
In binary tree, a node is visible only if its value is the largest one on the path from the root to itself.
Print out all the visible nodes in the tree.

### Example
* Input: 
  ```
       2
      / \
     7   1
   / \   / \
  6   5 3   4
  ```
  * Output: [2, 7, 3, 4]

## Solution: DFS recursion

### Complexity
* Time: O(n), n is the number of nodes in the tree
* Space: O(height of tree), call stack size

### Java
```java
public class Solution {
    public void printVisibles(TreeNode root) {
        if (root == null) return;
        dfs(root, Integer.MIN_VALUE);
    }
    
    private void dfs(TreeNode node, int curPathMax) {
        if (node == null) {
            return;
        }
        
        if (node.val > curPathMax) {
            System.out.println(node.val);
            curPathMax = node.val;
        }
        
        dfs(node.left, curPathMax);
        dfs(node.right, curPathMax);
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
