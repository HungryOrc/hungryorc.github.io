---
title: "Sum Root to Leaf Digits in a Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_sum_root_to_leaf_digits.html
folder: algorithm
toc: false
---

## Description
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.
An example is the root-to-leaf path 1->2->3 which represents the number 123.
Find the total sum of all root-to-leaf numbers.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```
Note: A leaf is a node with no children.

### Example
* Input: 
  ```
      4
     / \
    9   0
   / \
  5   1
  ```
  * Output: 495 + 491 + 40 = 1026

## Solution 1：DFS recursion，速度 前1%
直接就把各个数算出来！不要把每个list of integers搞出来之后再parse各个lists！那样也能做，但慢很多

### Complexity
* Time: O(n)，n是nodes的个数 <=== ？？？
* Space: O(tree height)，tree height即call stack的层数 <=== ？？？

### Java
```java
class Solution {
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] sum = new int[1];
        dfs(root, 0, sum);
        return sum[0];
    }
    
    private void dfs(TreeNode node, int curNum, int[] sum) {
        if (node.left == null && node.right == null) {
            curNum *= 10;
            curNum += node.val;
            sum[0] += curNum;
            return;
        }
        
        curNum *= 10;
        curNum += node.val;
        if (node.left != null) {
            dfs(node.left, curNum, sum);
        }
        if (node.right != null) {
            dfs(node.right, curNum, sum);
        }
    }
}
```

## Reference
* [Sum Root to Leaf Numbers [LeetCode]](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)

{% include links.html %}
