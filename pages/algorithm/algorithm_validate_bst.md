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
        if (node == null) {
            return true;
        }
        if (node.val <= min || node.val >= max) { // 据说最严格的BST里不允许出现重复的值
            return false;
        }
      
        return (isBST(node.left, min, node.val) && isBST(node.right, node.val, max));
        // 特别注意：这么做比先得到 leftIsBST 和 rightIsBST，再把两个 boolean && 在一起 要 好 很 多！
        // 因为 return(left && right)的写法，如果左半边==false，则右半边就不用做了
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
