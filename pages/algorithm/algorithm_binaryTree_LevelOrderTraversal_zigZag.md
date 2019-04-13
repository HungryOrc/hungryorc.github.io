---
title: "Binary Tree Level Order Zig Zag Traversal"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTree_LevelOrderTraversal_zigZag.html
folder: algorithm
toc: false
---

## Description
Get the list of keys in a given binary tree layer by layer in zig-zag order. For example for the following tree:
```
        5
      /    \
    3        8
  /   \        \
 1     4        11
```
the result is [5, 3, 8, 11, 4, 1].
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
见上文

## Solution：用一个Deque，挺巧妙的
用 Deque 两头进出。另外也需要一个左右方向的boolean flag

### Complexity
* Time: O(n)
* Space: O(n)，deque的size

### Java
代码看起来不短，但逻辑其实挺简明
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Deque<TreeNode> deque = new ArrayDeque<>();
        deque.addLast(root);
        boolean leftToRight = true;
        
        while (!deque.isEmpty()) {
            int size = deque.size();
            List<Integer> list = new ArrayList<>();
            
            if (leftToRight) {
                for (int i = 0; i < size; i++) {
                    TreeNode cur = deque.pollFirst();
                    list.add(cur.val);
                    if (cur.left != null) {
                        deque.addLast(cur.left);
                    }
                    if (cur.right != null) {
                        deque.addLast(cur.right);
                    }
                }
            } else {
                for (int i = 0; i < size; i++) {
                    TreeNode cur = deque.pollLast();
                    list.add(cur.val);
                    if (cur.right != null) {
                        deque.addFirst(cur.right);
                    }
                    if (cur.left != null) {
                        deque.addFirst(cur.left);
                    }
                }
            }
            result.add(list);
            leftToRight = !leftToRight;
        }
        return result;
    }
}
```

## Reference
* [Binary Tree Zigzag Level Order Traversal [LeetCode]](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

{% include links.html %}
