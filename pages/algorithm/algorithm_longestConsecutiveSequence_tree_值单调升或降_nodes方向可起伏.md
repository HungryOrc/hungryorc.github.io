---
title: "Longest Consecutive Sequence in Binary Tree, 值单调升或降，nodes方向可起伏"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longestConsecutiveSequence_tree_值单调升或降_nodes方向可起伏.html
folder: algorithm
toc: false
---

## Description
* tree里的 consecutive sequence 的意思是 **位置相邻** 且 **value连续变化(只差1)** 的nodes
  * 这和 array 里的 consecutive sequence 的意思是不一样的，array里是要求 **位置不一定相邻**，**value连续变化(只差1)** 的elements
* “值单调升或降”的意思是这里的 consecutive sequence 里的值必须是后一个比前一个大1，或者后一个比前一个小1。要么一直增大，要么一直减少
* node方向可起伏，即 可以从parent到child，也可以从child到parent，也可以从child到parent再到child

Given a binary tree, you need to find the length of Longest Consecutive Path in Binary Tree.

Especially, this path can be either increasing or decreasing. For example, [1,2,3,4] and [4,3,2,1] are both considered valid, but the path [1,2,4,3] is not valid. On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

Note: All the values of tree nodes are in the range of [-1e7, 1e7].
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
* Input: 
  ```
        1
       / \
      2   3
  ```
  * Output: 2, The longest consecutive path is [1, 2] or [2, 1].
* Input: 
  ```
        2
       / \
      1   3
  ```
  * Output: 3, The longest consecutive path is [1, 2, 3] or [3, 2, 1].

## Solution 1：DFS recursion，速度前5%

### Complexity
* Time: O(n), n 是tree里nodes的个数
* Space: O(tree height), call stack size 

### Java
```java
class Solution {
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] maxLen = {0};
        dfs(root, Integer.MIN_VALUE, 0, maxLen);
        return maxLen[0];
    }
    
    private void dfs(TreeNode node, int prevVal, int prevLen, int[] maxLen) {
        if (node == null) {
            return;
        }
        
        int curLen = 1;
        int curVal = node.val;
        if (prevVal + 1 == curVal) {
            curLen = prevLen + 1;
        }
        maxLen[0] = Math.max(curLen, maxLen[0]);
        
        dfs(node.left, curVal, curLen, maxLen);
        dfs(node.right, curVal, curLen, maxLen);
    }
}
```

另一种非常相似的实现方式，也是 DFS recursion：
```java
class Solution {
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return dfs(root, Integer.MIN_VALUE, 0);
    }

    private int dfs(TreeNode node, int prevVal, int prevLen) {
        if (node == null) {
            return prevLen;
        }
        
        int curLen = 1;
        if (prevVal + 1 == node.val) {
            curLen = prevLen + 1;
        }

        int leftLen = dfs(node.left, node.val, curLen);
        int rightLen = dfs(node.right, node.val, curLen);
       
        return Math.max(curLen, Math.max(leftLen, rightLen));
    }
}
```

## Solution 2：DFS recursion，用了自定义的 result class，思路晦涩，速度很慢，后20%

### Complexity
* Time: O(n), n 是tree里nodes的个数
* Space: O(tree height), call stack size 

### Java
```java
class ResultType {
    int maxInSubtree;
    int maxFromRoot;
    public ResultType(int maxS, int maxR) {
        this.maxInSubtree = maxS;
        this.maxFromRoot = maxR;
    }
}

public class Solution {
    public int longestConsecutive(TreeNode root) {
        return dfs(root).maxInSubtree;
    }
    
    private ResultType dfs(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0);
        }
        
        ResultType left = dfs(root.left);
        ResultType right = dfs(root.right);
        
        // 1 stands for the root itself.
        ResultType result = new ResultType(0, 1);
        
        if (root.left != null && root.val + 1 == root.left.val) {
            result.maxFromRoot = Math.max(
                result.maxFromRoot,
                left.maxFromRoot + 1
            );
        }
        
        if (root.right != null && root.val + 1 == root.right.val) {
            result.maxFromRoot = Math.max(
                result.maxFromRoot,
                right.maxFromRoot + 1
            );
        }
        
        result.maxInSubtree = Math.max(
            result.maxFromRoot,
            Math.max(left.maxInSubtree, right.maxInSubtree)
        );
        
        return result;
    }
}
```

## Reference
* [Binary Tree Longest Consecutive Sequence
 [LeetCode]](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/description/)

{% include links.html %}
