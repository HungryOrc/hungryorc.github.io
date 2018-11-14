---
title: "Binary Tree Preorder Traversal"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_preorder.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, return the preorder traversal of its nodes' values.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

先root，再左子树，再右子树

### Example
* Input: Given binary tree {1,#,2,3}:
  ```
   1
    \
     2
    /
   3
  ```
  * Output: [1, 2, 3]

## Solution 1: Iteration with a Stack

### Complexity
* Time: O(n)
* Space: O(n), size of the stack

### Java
```java
class Solution {
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        ArrayList<Integer> preorder = new ArrayList<>();
        
        if (root == null) {
            return preorder;
        }
        
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            preorder.add(cur.val);
            
            // 因为遍历的时候要先走左边，所以入栈的时候要先入右子！因为先入后出
            if (cur.right != null) {
                stack.push(cur.right);
            }
            if (cur.left != null) {
                stack.push(cur.left);
            }
        }
        return preorder;
    }
}
```

## Solution 2: Recursion

### Complexity
* Time: O(n)
* Space: O(n), call stack的层数

### Java
```java
class Solution {
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        preorder(root, result);
        return result;
    }
    
    private void preorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        
        result.add(root.val);
        
        preorder(root.left, result);
        preorder(root.right, result);
    }
}
```

## Solution 3: Divide & Conquer, like Recursion without a helper function
这种做法非常好玩！

### Complexity
* Time: O(n)
* Space: O(n), call stack的层数

### Java
```java
class Solution {
    public ArrayList<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        // Divide
        ArrayList<Integer> leftSubtreeValues = preorderTraversal(root.left);
        ArrayList<Integer> rightSubtreeValues = preorderTraversal(root.right);
        
        // Conquer
        result.add(root.val);
        result.addAll(leftSubtreeValues);
        result.addAll(rightSubtreeValues);
        return result;
    }
}
```

## Reference
* [Binary Tree Preorder Traversal [LeetCode]](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

{% include links.html %}
