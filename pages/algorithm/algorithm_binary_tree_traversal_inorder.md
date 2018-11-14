---
title: "Binary Tree Inorder Traversal"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_inorder.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, return the inrder traversal of its nodes' values.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

先左子树，再root，再右子树

### Example
* Input: Given binary tree {1,#,2,3}:
  ```
   1
    \
     2
    /
   3
  ```
  * Output: [1, 3, 2]

## Solution 1: Recursion

### Complexity
* Time: O(n)
* Space: O(n), call stack的层数

### Java
```java
class Solution {
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }
   
    private void inorder(TreeNode root, ArrayList<Integer> result) {
        if (root == null) {
            return;
        }
       
        inorder(root.left, result);
        result.add(root.val);
        inorder(root.right, result);
    }
}
```

## Solution 2: Divide & Conquer, like Recursion without a helper function
这种做法非常好玩！

### Complexity
* Time: O(n)
* Space: O(n), call stack的层数

### Java
```java
class Solution {
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
       
        // Divide 
        ArrayList<Integer> leftSubtreeValues = inorderTraversal(root.left);
        ArrayList<Integer> rightSubtreeValues = inorderTraversal(root.right);
       
        // Conquer
        result.addAll(leftSubtreeValues);
        result.add(root.val);
        result.addAll(rightSubtreeValues);
       
        return result;
    }
}
```

## Solution 1: Iteration with a Stack，我的独创方法hoho

### Complexity
* Time: O(n)
* Space: O(n), size of the stack

### Java
```java
class Solution {
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        
        if (root == null) {
            return result;
        }
        
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
           
            // 要么，这个node是leaf
            // 要么，这个node的左右子都已被纳入stack了
            if (cur.left == null && cur.right == null) {
                result.add(cur.val);
                continue;
            } 
            
            // 先把右边的放进stack，即最后处理右子树
            if (cur.right != null) {
                stack.push(cur.right);
            }

            // 把中间的也就是自己放进stack
            stack.push(cur);

            // 最后把左边的放进stack，即下一步就要先处理左子树
            if (cur.left != null) {
                stack.push(cur.left);
            }

            // 本node的历史使命完成了。左右都剪掉，以利于将来被处理
            cur.left = null;
            cur.right = null;
        }
        return result;
    }
}
```

## Reference
* [Binary Tree Inorder Traversal [LeetCode]](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

{% include links.html %}
