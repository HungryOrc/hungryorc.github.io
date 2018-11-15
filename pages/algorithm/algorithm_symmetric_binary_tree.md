---
title: "Check if a Binary Tree is Symmetric"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_symmetric_binary_tree.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
```java
public class TreeNode {
    public int key;
    public TreeNode left, right;
    public TreeNode(int key) {
        this.key = key;
    }
}
```

### Follow up
分别用recursion以及iteration做

### Example
略

## Solution 1: Recursion，前 1% 的速度

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree)，即call stack的层数

### Java
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        return isSym(root.left, root.right);
    }
    
    private boolean isSym(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } else if (left == null || right == null) {
            return false;
        }
        
        if (left.val != right.val) {
            return false;
        }
        
        return isSym(left.left, right.right) && isSym(left.right, right.left);
    }
}
```

## Solution 2: Iteration，用两个queue或两个stack，前 1% 速度
用两个queue或者stack分别装整个大树的左半部分和右半部分。用两个queue或用两个stack的做法是同理的。

关键在于，两边放入node的次序要正好相反：
* 左边放入 左左时，右边放入 右右
* 左边放入 左右时，右边放入 右左

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(1)

### Java
下面的做法是用了2个queue。如果要用2个stack也是很类似的代码
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Queue<TreeNode> ql = new LinkedList<>();
        Queue<TreeNode> qr = new LinkedList<>();
        
        ql.offer(root.left);
        qr.offer(root.right);
        
        while(!ql.isEmpty() || !qr.isEmpty()) {
            TreeNode l = ql.poll();
            TreeNode r = qr.poll();
            
            if (l == null && r == null) {
                continue;
            } else if (l == null && r != null) {
                return false;
            } else if (l != null && r == null) {
                return false;
            }
            
            if (l.val != r.val) {
                return false;
            }
            
            // 关键在下面几句话
            // input one pair
            ql.offer(l.left);
            qr.offer(r.right);
            // input the other pair
            ql.offer(l.right);
            qr.offer(r.left);
        }
        
        if (!ql.isEmpty() || !qr.isEmpty()) {
            return false;
        }
        return true;
    }
}
```

## Reference
* [Symmetric Tree [LeetCode]](https://leetcode.com/problems/symmetric-tree/description/)

{% include links.html %}
