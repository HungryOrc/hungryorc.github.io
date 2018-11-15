---
title: "Exactly Same Binary Tree"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_same_binary_tree.html
folder: algorithm
toc: false
---

## Description
判断两个树是否完全相同（所有位置所有值都相同）。

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.
```java
public class TreeNode {
    int key;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
* Input: Given the following 2 binary trees:
  ```
    1         1
   / \       / \
  2   3     2   3
  ```
  * Output: true，意思是他们两完全相同
* Input:
  ```
    1         1
   / \       / \
  3   2     2   3
  ```
  * Output: false

## Solution 1: Recursion

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree), call stack的层数

### Java
```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        }
        
        if (p.val != q.val) {
            return false;
        }
        
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

## Solution 2: Iteration with 2 Queues，我自己的方法

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(n), queue里装的一“层”的tree nodes的个数可能达到n的量级

### Java
```java
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) { 
        Queue<TreeNode> qp = new LinkedList<>();
        Queue<TreeNode> qq = new LinkedList<>();
        qp.offer(p);
        qq.offer(q);
        
        while (!qp.isEmpty() && !qq.isEmpty()) {
            TreeNode curp = qp.poll();
            TreeNode curq = qq.poll();
            
            if (curp == null && curq == null) {
                continue;
            } else if (curp == null || curq == null) {
                return false;
            }
            
            if (curp.val != curq.val) {
                return false;
            }

            qp.offer(curp.left);
            qp.offer(curp.right);
                        
            qq.offer(curq.left);
            qq.offer(curq.right);
        }
        
        if (qp.isEmpty() && qq.isEmpty()) {
            return true;
        } 
        return false;
    }
}
```

## Solution 3: Iteration with 2 Stacks，我自己的方法

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree), 因为stack里装的东西相当于 DFS

### Java
```java
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // define 2 stacks for p and q respectively
        Deque<TreeNode> sp = new ArrayDeque<>();
        Deque<TreeNode> sq = new ArrayDeque<>();
        
        sp.push(p);
        sq.push(q);
        
        while (!sp.isEmpty() && !sq.isEmpty()) {
            TreeNode curp = sp.pop();
            TreeNode curq = sq.pop();
            
            if (curp == null && curq == null) {
                continue;
            } else if (curp == null || curq == null) {
                return false;
            }
            
            if (curp.val != curq.val) {
                return false;
            }
            
            System.out.println(curp.left);
            sp.push(curp.left);
            sp.push(curp.right);
            
            sq.push(curq.left);
            sq.push(curq.right);
        }
        
        if (sp.isEmpty() && sq.isEmpty()) {
            return true;
        }
        return false;
    }
}
```

## Reference
* [Same Tree [LeetCode]](https://leetcode.com/problems/same-tree/description/)

{% include links.html %}
