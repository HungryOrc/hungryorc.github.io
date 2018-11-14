---
title: "Check if a Binary Tree is Balanced"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_balanced_binary_tree.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, determine if it is height-balanced.

A height-balanced binary tree is defined as: "A binary tree in which the depth of the two subtrees of every node never differ by more than 1."

```java
public class TreeNode {
    public int key;
    public TreeNode left, right;
    public TreeNode(int key) {
        this.key = key;
    }
}
```

### Example
略

## Solution 1: Recursion，用了一个特殊的helper function
见下面的代码，这个helper function的特殊之处在于：
* 如果以当前node为root的树是balanced，本函数返回此树的height，
* 如果不是balanced，则本函数返回 -1
    
### Complexity
* Time: O(n)，n是tree node的个数 <==== 对么 ？？？
* Space: O(height of tree)，即call stack的层数 <=== 对么 ？？？

### Java
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeightOrNegative(root) != -1;
    }
    
    // 这个算法的关键 在于这个helper function！
    private int getHeightOrNegative(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        int leftResult = getHeightOrNegative(node.left);
        // 提前返回 -1，减支提速
        if (leftResult == -1) {
            return -1;
        }
        
        int rightResult = getHeightOrNegative(node.right);
        // 提前返回 -1，减支提速
        if (rightResult == -1) {
            return -1;
        }
        
        if (Math.abs(leftResult - rightResult) > 1) {
            return -1;
        }
        
        return 1 + Math.max(leftResult, rightResult);
    }
}
```

## Solution 2: Recursion，用一个function求height，另一个查balanced。慢
见下面的代码，这个方法最符合直觉，但速度慢。因为：

对于root来说，run一次isBalanced的话：
* 首先要求一次leftH和rightH，这两个加在一起就是n的时间。因为getHeight跑一次就是相应的(子)树里的nodes的个数
* 然后还要对root.left和root.right分别跑一次isBalanced：
  * 对root.left跑一次isBalanced，这里面就包括求left的leftH和left的rightH，这样就是n/2的时间
  * 同理，对root.right跑一次isBalanced，也要n/2的时间
  * 那么这两个isBalanced函数里的leftH和rightH的部分就一共要用n的时间
  * 这两个isBalanced函数还要继续对 left.left, left.right, right.left, right.right 跑isBalaned...
* 可以想到，对于整棵树里的每一“层”，isBalanced函数里求leftH和rightH的部分都一共需要n的时间，isBalanced函数的余下部分所需的时间就下沉到下一层
所需的时间
* 所以总的来说，一共需要 n * (height of tree) 的时间

### Complexity
* Time: O(n * height of tree)，理想的情况下是 O(nlogn)
* Space: O(height of tree)，即call stack的层数

### Java
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
        
        return Math.abs(leftH - rightH) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }
 
    private int getHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        return Math.max(getHeight(root.left), getHeight(root.right)) + 1;
    }   
}
```

## Solution 3: 九章的方法，搞一个return type class
Ref: http://www.jiuzhang.com/solutions/balanced-binary-tree/

### Complexity
* Time: 应该和方法1是一样的，O(n)  <=== 对么 ？？？
* Space: 应该和方法1是一样的，O(height of tree) <=== 对么 ？？？

### Java
```java
class BalanceAndDepth {
    public boolean isBalanced;
    public int maxDepth;
    public BalanceAndDepth (boolean isBalanced, int maxDepth) {
        this.isBalanced = isBalanced;
        this.maxDepth = maxDepth;
    }
}
    
public class Solution {    
    public boolean isBalanced(TreeNode root) {
        return checkBalanceAndDepth(root).isBalanced;
    }
    
    private BalanceAndDepth checkBalanceAndDepth(TreeNode curNode) {
        if (curNode == null) {
            return new BalanceAndDepth(true, 0);
        }
        
        BalanceAndDepth leftSubtreeStatus = checkBalanceAndDepth(curNode.left);
        if (!leftSubtreeStatus.isBalanced) {
            return new BalanceAndDepth(false, -1); // the actual depth does not matter now
        }
        
        BalanceAndDepth rightSubtreeStatus = checkBalanceAndDepth(curNode.right);
        if (!rightSubtreeStatus.isBalanced) {
            return new BalanceAndDepth(false, -1); // the actual depth does not matter now
        }
        
        // if the current node itself is not balanced
        if (Math.abs(leftSubtreeStatus.maxDepth - rightSubtreeStatus.maxDepth) > 1) {
            return new BalanceAndDepth(false, -1); // the actual depth does not matter now
        }
        
        return new BalanceAndDepth(true, 
            Math.max(leftSubtreeStatus.maxDepth, rightSubtreeStatus.maxDepth) + 1);
    }
}
```

## Reference
* [Balanced Binary Tree [LeetCode]](https://leetcode.com/problems/balanced-binary-tree/description/)

{% include links.html %}
