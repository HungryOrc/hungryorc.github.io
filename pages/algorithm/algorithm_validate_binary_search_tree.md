---
title: "Check if a Tree is a Binary Search Tree"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validate_binary_search_tree.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

```java
public class TreeNode {
    int key;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
略

## Solution: Recursion，速度 前1%

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree), call stack的层数

### Java
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }        
        
        // 按本题题意，node的val可以取到int数据类型的上下限值，
        // 所以这里不得不用long型的上下限作为不可达到的边界
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean validate(TreeNode node, long min, long max) {
        if (node == null) {
            return true;
        }
        // 按本题题意，BST里面不可以存在相同val的nodes，
        // 所以一个node的val如果和它的parent node的val相同，也要返回false
        if (node.val >= max || node.val <= min) {
            return false;
        }
        
        return validate(node.left, min, node.val) && 
               validate(node.right, node.val, max);
    }
}
```

## Solution 2: Iteration
一个不含重复元素的BST，被in-order遍历的话，会形成一个单调上升的序列。
如此，我们就可用stack做一个中序遍历，把结果放到一个 ArrayList 里，再验证是不是每个元素都比前一个大

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Stack<TreeNode> nodeStack = new Stack<>();
        nodeStack.push(root);
        ArrayList<Integer> allNodesValInOrder = new ArrayList<>();
        
        while (!nodeStack.isEmpty()) {
            TreeNode curNode = nodeStack.pop();
            
            // 当前node是leaf；或当前node的左右children都被放入stack了，
            // 则意味着当前node之前已被处理过了。那么当前node就可以被放入list了
            if (curNode.left == null && curNode.right == null) {
                allNodesValInOrder.add(curNode.val);
                continue;
            }
            
            // in-order
            if (curNode.right != null) {
                nodeStack.push(curNode.right);
            }
            nodeStack.push(curNode); // 对于curNode，刚才pop出来，现在又push回去了！
            if (curNode.left != null) {
                nodeStack.push(curNode.left);
            }
            
            curNode.left = null;
            curNode.right = null;
        }
        
        for (int i = 1; i < allNodesValInOrder.size(); i++) {
            if (allNodesValInOrder.get(i) <= allNodesValInOrder.get(i - 1)) {
                return false;
            }
        }
        return true;
    }
}
```

## Solution 3: Recursion + Custom Result Class。看看就好，别用了
Ref: http://www.jiuzhang.com/solutions/validate-binary-search-tree/

### Complexity
* Time: O(n)
* Space: O(tree height)

### Java
```java
class ResultType {
    boolean isBst;
    int maxValue, minValue;

    ResultType(boolean isBst, int maxValue, int minValue) {
        this.isBst = isBst;
        this.maxValue = maxValue;
        this.minValue = minValue;
    }
}

public class Solution {

    public boolean isValidBST(TreeNode root) {
        ResultType rt = validateHelper(root);
        return rt.isBst;
    }
    
    // make this helper function, so it can return a ResultType
    private ResultType validateHelper(TreeNode root) {
        if (root == null) {
            // for max, set Integer.MIN_VALUE; for min, set Integer.MAX_VALUE,
            // the reason is as the last statement in this function:
            //         return new ResultType(true,
            //                  Math.max(root.val, right.maxValue),
            //                  Math.min(root.val, left.minValue));
            return new ResultType(true, Integer.MIN_VALUE, Integer.MAX_VALUE);
        }

        ResultType left = validateHelper(root.left);
        ResultType right = validateHelper(root.right);

        if (!left.isBst || !right.isBst) {
            // if isBst is false then minValue and maxValue are useless
            return new ResultType(false, 0, 0);
        }

        if (root.left != null && left.maxValue >= root.val || 
            root.right != null && right.minValue <= root.val) {
            return new ResultType(false, 0, 0);
        }

        return new ResultType(true,
                              Math.max(root.val, right.maxValue),
                              Math.min(root.val, left.minValue));
    }
}
```

## Reference
* [Validate Binary Search Tree [LeetCode]](https://leetcode.com/problems/validate-binary-search-tree/description/)

{% include links.html %}
