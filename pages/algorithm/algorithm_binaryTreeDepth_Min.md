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

## Solution 1: 一种Recursion，base case写在 null node（leaf node再往下一层）上

### Complexity
* Time: O(n)
* Space: O(tree height)，即 call stack 的层数

### Java
```java
public class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        if (root.left != null && root.right != null) {
            return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
        } else if (root.left != null) {
            return minDepth(root.left) + 1;
        } else { // root.right != null
            return minDepth(root.right) + 1;
        }
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

## Solution 2: 另一种 Recursion

### Complexity
* Time: O(n)
* Space: O(n)，queue size

### Java
```java
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        int curDepth = 0;
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            curDepth++;
            
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (cur.left != null) {
                    queue.offer(cur.left);
                }
                if (cur.right != null) {
                    queue.offer(cur.right);
                }
            }
        }
        return curDepth;
    }
}
```

## Solution 3: DFS，用2个Stack，一个存Node，一个存对应于当前Node的Depth。这个方法看看就好


### Complexity
* Time: O(n)
* Space: O(n)，queue size

### Java
```java
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        Deque<TreeNode> nodeStack = new ArrayDeque<>();
        Deque<Integer> depthStack = new ArrayDeque<>();
        
        // 初始
        int maxDepth = 1;
        nodeStack.push(root);
        depthStack.push(1);
        
        while (!nodeStack.isEmpty()) {
            TreeNode curNode = nodeStack.pop();
            int curDepth = depthStack.pop();
            
            maxDepth = Math.max(maxDepth, curDepth);
        
            if (curNode.left != null) {
                nodeStack.push(curNode.left);
                depthStack.push(curDepth + 1); // 比较有趣的处理方式
            }
            if (curNode.right != null) {
                nodeStack.push(curNode.right);
                depthStack.push(curDepth + 1);
            }
        }
        return maxDepth;
    }
}
```

## Reference
* [Minimum Depth of Binary Tree [LeetCode]](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

{% include links.html %}
