---
title: "Minimum Depth of Binary Tree (Shortest Path from Root to Leaves)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTreeDepth_Min.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
略

## Solution 1: 一种 DFS Recursion，base case写在 null node（leaf node再往下一层）上

### Complexity
* Time: O(n)
* Space: O(tree height)，即 call stack 的层数

### Java
```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        if (root.left == null) {
            return minDepth(root.right) + 1;
        } else if (root.right == null) {
            return minDepth(root.left) + 1;
        }
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```
注意！下面这么写是不行的！因为 如果一个node的left child为null，不能认为path在这个node就结束了，它可能还有right child呢。
left child为null并不形成一个断的支路，而是成就了right child成为唯一的一条路
```java
// 错：
public class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```

## Solution 2: 另一种 DFS Recursion，base case写在 leaf node上

### Complexity
* Time: O(n)
* Space: O(tree height)，即 call stack 的层数

### Java
```java
public class Solution {
    public int minDepth(TreeNode root) {
        if (root.left == null && root.right == null) { // 前提：整个树的root不是null
            return 1;
        }
        
        // 下面这些代码和上面的base case写在leaf node上的方法是一样的
        if (root.left == null) {
            return minDepth(root.right) + 1;
        } else if (root.right == null) {
            return minDepth(root.left) + 1;
        }
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```

## Solution 3: BFS Iteration

### Complexity
* Time: O(n)
* Space: O(n)，queue size

### Java
```java
public class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int curDepth = 0;
        Queue<TreeNode> nodeQueue = new LinkedList<TreeNode>();
        nodeQueue.offer(root);
        
        while (!nodeQueue.isEmpty()) {
            int size = nodeQueue.size();
            curDepth ++;
            
            for (int i = 0; i < size; i++) {
                TreeNode curNode = nodeQueue.poll();
                
                // 找到一个leaf，那么此时找 min depth 的工作就可以结束了
                if (curNode.left == null && curNode.right == null) {
                    return curDepth;
                }
                
                if (curNode.left != null) {
                    nodeQueue.offer(curNode.left);
                }
                if (curNode.right != null) {
                    nodeQueue.offer(curNode.right);
                }
            }
        }
        return -1; // actually we will never reach here
    }
}
```

## Reference
* [Minimum Depth of Binary Tree [LeetCode]](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

{% include links.html %}
