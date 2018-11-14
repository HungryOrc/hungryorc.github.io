---
title: "Height of Binary Tree"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_height_of_binary_tree.html
folder: algorithm
toc: false
---

## Description
Get the height of the binary tree. Root count as height 1. 
```java
public class TreeNode {
    public int key;
    public TreeNode left, right;
    public TreeNode(int key) {
        this.key = key;
    }
}
```

### Example
略

## Solution


### Complexity
* Time: O(n)
* Space: O(height of tree)，worst O(n)，best O(logn)

### Java
```java
public class Solution {
    public int findHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
      
        return 1 + Math.max(findHeight(root.left), 
                            findHeight(root.right));
    }
}
```

## Reference
* [Height of Binary Tree [LaiCode]](https://app.laicode.io/app/problem/60)

{% include links.html %}
