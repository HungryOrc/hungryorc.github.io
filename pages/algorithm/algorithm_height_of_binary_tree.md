---
title: "Height of Binary Tree"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_height_of_binary_tree.html
folder: algorithm
toc: false
---

## Description
Get the height of the binary tree. Root count as height 1. 
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

## Solution 1: Recursion，这个方法较好，用Iteration的话较累赘

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(height of tree)，因为call stack的层数等于height of tree。worst O(n)，best O(logn)

### Java
```java
public class Solution {
    public int findHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
      
        return 1 + Math.max(findHeight(root.left), 
                            findHeight(root.right));
    }
}
```

## Solution 2: Iteration with a Queue
代码比recursion的长一点，好处是空间消耗低，因为没有多层的call stack

### Complexity
* Time: O(n)，n是tree node的个数
* Space: O(1)

### Java
```java
public class Solution {
    public int findHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
      
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int height = 0;
      
        while (!queue.isEmpty()) {
            int size = queue.size();
            height++;
          
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
        return height;
    }
}
```

## Reference
* [Height of Binary Tree [LaiCode]](https://app.laicode.io/app/problem/60)

{% include links.html %}
