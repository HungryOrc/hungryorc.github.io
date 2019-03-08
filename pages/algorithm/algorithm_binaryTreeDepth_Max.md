---
title: "Maximum Depth of Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTreeDepth_Max.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, find its maximum depth.
The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
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

## Solution 1: Recursion，Divide and Conquor，实测速度很快

### Complexity
* Time: O(n)
* Space: O(tree height)，即 call stack 的层数

### Java
```java
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
```

## Solution 2: BFS

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
* [Maximum Depth of Binary Tree [LeetCode]]()

{% include links.html %}
