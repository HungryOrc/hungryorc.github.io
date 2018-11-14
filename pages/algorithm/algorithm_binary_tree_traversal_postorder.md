---
title: "Binary Tree Postorder Traversal"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_postorder.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, return the postorder traversal of its nodes' values.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

先左子树，再右子树，最后root

### Follow up
Recursion的做法很简单，能用iteration来做才是nb哈哈

### Example
* Input: Given binary tree {1,#,2,3}:
  ```
   1
    \
     2
    /
   3
  ```
  * Output: [3, 2, 1]


## Solution 1: Iteration with a Stack，我的独创方法！应follow up question的要求

### Complexity
* Time: O(n)
* Space: O(n), size of the stack

### Java
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            
            // 要么，这个node是leaf
            // 要么，这个node的左右子都已被纳入stack了
            if (cur.left== null && cur.right == null) {
                result.add(cur.val);
                continue;
            }
            
            // 先把中间的也就是自己放进stack
            stack.push(cur);
            // 再把右边的放进stack，即处理右子树
            if (cur.right != null) {
                stack.push(cur.right);
            }
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

## Solution 2: Recursion

### Complexity
* Time: O(n)
* Space: O(n), call stack的层数

### Java
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        postorder(root, result);
        return result;
    }
    
    private void postorder(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        
        postorder(node.left, result);
        postorder(node.right, result);
        result.add(node.val);
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        List<Integer> leftSubtreeValues = postorderTraversal(root.left);
        List<Integer> rightSubtreeValues = postorderTraversal(root.right);
        
        result.addAll(leftSubtreeValues);
        result.addAll(rightSubtreeValues);
        result.add(root.val);
        return result;
    }
}
```

## Reference
* [Binary Tree Post Traversal [LeetCode]](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

{% include links.html %}
