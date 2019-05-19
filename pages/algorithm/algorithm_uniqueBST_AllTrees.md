---
title: "All Unique Binary Search Trees"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_uniqueBST_AllTrees.html
folder: algorithm
toc: false
---

## Description
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

Return a list of the root nodes of all these unique BST's.

### Example
* Input: 3
  * Output: 
  ```
  [
    [1,null,3,2],
    [3,2,null,1],
    [3,1,null,null,2],
    [2,1,3],
    [1,null,2,null,3]
  ]
  ```
  ```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
  ```

## Solution：DP，速度前5%
参考一下 Unique Binary Search Trees I 的做法

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public class Solution {
    public static List<TreeNode> generateTrees(int n) {
        List<TreeNode>[] result = new List[n + 1];
        result[0] = new ArrayList<TreeNode>();
        if (n == 0) {
            return result[0];
        }

        result[0].add(null);
        for (int len = 1; len <= n; len++) {
            result[len] = new ArrayList<TreeNode>();
            for (int j = 0; j < len; j++) {
                for (TreeNode nodeL : result[j]) {
                    for (TreeNode nodeR : result[len - j - 1]) {
                        TreeNode node = new TreeNode(j + 1);
                        node.left = nodeL;
                        node.right = clone(nodeR, j + 1);
                        result[len].add(node);
                    }
                }
            }
        }
        return result[n];
    }

    private static TreeNode clone(TreeNode n, int offset) {
        if (n == null) {
            return null;
        }
        TreeNode node = new TreeNode(n.val + offset);
        node.left = clone(n.left, offset);
        node.right = clone(n.right, offset);
        return node;
    }
}
```

## Reference
* [Unique Binary Search Trees II [LeetCode]](https://leetcode.com/problems/unique-binary-search-trees-ii/description/)

{% include links.html %}
