---
title: "Range Sum of BST"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rangeSum_nodesInBST.html
folder: algorithm
toc: false
---

## Description
Given the root node of a binary search tree, return the sum of values of all nodes with value between L and R (inclusive).
The binary search tree is guaranteed to have unique values.

Note:
* The number of nodes in the tree is at most 10,000.
* The final answer is guaranteed to be less than 2^31.

### Example
* Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
  * Output: 32

## Solution: 我的实现，速度前1%
思路清爽干脆

### Complexity
* Time: O(n)，n是tree里nodes的个数
* Space: O(tree height)，stack的层数

### Java
```java
class Solution {
    public int rangeSumBST(TreeNode root, int lb, int ub) {
        if (root == null || lb > ub) return 0;
        
        int curVal = root.val;
        
        if (curVal >= lb && curVal <= ub) {
            return curVal + rangeSumBST(root.left, lb, curVal - 1)
                          + rangeSumBST(root.right, curVal + 1, ub);
        }
        else if (curVal < lb) {
            return rangeSumBST(root.right, lb, ub);
        }
        else { // curVal > ub
            return rangeSumBST(root.left, lb, ub);
        }
    }
}
```

## Reference
* [Range Sum of BST [LeetCode]](网址放在这里)

{% include links.html %}
