---
title: "Check if a Binary Tree is Balanced"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_balanced_binary_tree.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, determine if it is height-balanced.

A height-balanced binary tree is defined as: "A binary tree in which the depth of the two subtrees of every node never differ by more than 1."

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

## Solution 1: Recursion，用了一个特殊的helper function
见下面的代码，这个helper function的特殊之处在于：
* 如果以当前node为root的树是balanced，本函数返回此树的height，
* 如果不是balanced，则本函数返回 -1
    
### Complexity
* Time: O(n)，n是tree node的个数 <==== 对么 ？？？
* Space: O(height of tree)，即call stack的层数 <=== 对么 ？？？

### Java
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeightOrNegative(root) != -1;
    }
    
    // 这个算法的关键 在于这个helper function！
    private int getHeightOrNegative(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        int leftResult = getHeightOrNegative(node.left);
        // 提前返回 -1，减支提速
        if (leftResult == -1) {
            return -1;
        }
        
        int rightResult = getHeightOrNegative(node.right);
        // 提前返回 -1，减支提速
        if (rightResult == -1) {
            return -1;
        }
        
        if (Math.abs(leftResult - rightResult) > 1) {
            return -1;
        }
        
        return 1 + Math.max(leftResult, rightResult);
    }
}
```

## Solution 2: Recursion，用一个function求height，另一个查balanced。慢
见下面的代码，这个方法最符合直觉，但速度慢。因为：

对于root来说，run一次isBalanced，首先要run一次left

### Complexity
* Time: O(n)，n是tree node的个数 <==== 对么 ？？？
* Space: O(height of tree)，即call stack的层数

### Java
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null)
            return true;
        
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
        
        return Math.abs(leftH - rightH) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }
 
    private int getHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        return Math.max(getHeight(root.left), getHeight(root.right)) + 1;
    }   
}
```




## Reference
* [Balanced Binary Tree [LeetCode]](https://leetcode.com/problems/balanced-binary-tree/description/)

{% include links.html %}
