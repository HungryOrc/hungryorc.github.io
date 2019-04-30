---
title: "Sum Root to Leaf Digits in a Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_pathSum_asDigits_rootToLeaf.html
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
        curNum *= 10;
        curNum += node.val;
        
        // 这里不要用 node == null 这样的结束条件！因为如果那样，就会导致
        // 对于每个leaf node，它的左右children都是null，那么这个leaf node会被算两次
        if (node.left == null && node.right == null) {
            sum[0] += curNum;
            return;
        }
        
        if (node.left != null) {
            dfs(node.left, curNum, sum);
        }
        if (node.right != null) {
            dfs(node.right, curNum, sum);
        }
    }
}
```

## Solution 2：DFS iteration，一种思路很清奇的做法

### Complexity
* Time: O(n)，n是nodes的个数 <=== ？？？
* Space: O(n)，stack size <=== ？？？

### Java
```java
class Solution {
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int sum = 0;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode curNode = stack.pop();
            int curVal = curNode.val;
            
            if (curNode.left != null) {
                curNode.left.val += curVal * 10;
                stack.push(curNode.left);
            }
            if (curNode.right != null) {
                curNode.right.val += curVal * 10;
                stack.push(curNode.right);
            }
            
            if (curNode.left == null && curNode.right == null) {
                sum += curNode.val;
            }
        }
        return sum;
    }
}
```

## Reference
* [Sum Root to Leaf Numbers [LeetCode]](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)

{% include links.html %}
