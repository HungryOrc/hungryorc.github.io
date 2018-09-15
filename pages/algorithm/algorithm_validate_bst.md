---
title: "Validate Binary Search Tree"
tags: [algorithm, binary_search_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validate_bst.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:
1. The left subtree of a node contains only nodes with keys less than the node's key.
2. The right subtree of a node contains only nodes with keys greater than the node's key.
3. Both the left and right subtrees must also be binary search trees.
4. A single node tree is a BST.

Definition of TreeNode:
```java
public class TreeNode {
    public int val;
    public TreeNode left, right;
    public TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}
```

### Example
略
    
## Solution 1: Recursion

### Complexity
* Time: O(n), n is the number of the TreeNodes in the tree. Because we need to walk through every node in the tree in the worst case.
* Space: O(height of the tree), because the number of call stacks equals to the number of tree height, and each call stack requires constant space.

### Java
```java
public class Solution {

    public boolean isValidBST(TreeNode root) {
        return isBST(root, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }  
    
    private boolean isBST(TreeNode node, int min, int max) {
        // 只要是 recursion，都千万记住 base case 千万别忘了 ！！！
        // 要是忘了就是 一发入魂 级别的 instant death ！！！
        if (node == null) {
            return true;
        }
        if (node.val <= min || node.val >= max) { // 最严格的BST里不允许出现重复的值
            return false;
        }
      
        return (isBST(node.left, min, node.val) && isBST(node.right, node.val, max));
        // 特别注意 ！！！这么做比先得到 leftIsBST 和 rightIsBST，再把两个 boolean && 在一起  好  很  多 ！！！
        // 这是因为，return(left && right)的写法，如果左半边==false，则右半边就不用做了 ！！！
        // 而分别先把left和right搞出来的写法，是一点懒都没法偷的！全部都得先算出来再说！
    }
}
```

## Solution 2: Iteration
一个不含重复元素的BST，被in-order遍历的话，会形成一个单调上升的序列。
如此，我们就可用stack做一个中序遍历，把结果放到一个 array list 里，再验证是不是每个元素都比前一个大

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
```

## Reference

{% include links.html %}
