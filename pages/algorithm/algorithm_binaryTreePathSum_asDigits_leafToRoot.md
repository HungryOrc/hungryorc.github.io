---
title: "Sum Root to Leaf Digits in a Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binaryTreePathSum_asDigits_leafToRoot.html
folder: algorithm
toc: false
---

## Description
Given a binary tree containing digits from 0-9 only, each leaf-to-root path could represent a number.
An example is the root-to-leaf path 1(root)->2->3(leaf) which represents the number 321.
Find the total sum of all leaf-to-root numbers.
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```
Note: A leaf is a node with no children.

### 近似题：从root sum 到 leaves，root是最高位
leetcode有这道近似题

### Example
* Input: 
  ```
      1
     /  \
    2    3
        / \
       5   4
        \
         6
  ```
  * Output: 21 + 6531 + 431 = 6983

## Solution：DFS recursion
直接就把各个数算出来！不要把每个list of integers搞出来之后再parse各个lists！那样也能做，但慢很多

### Complexity
* Time: O(n)，n是nodes的个数 <=== ？？？
* Space: O(tree height)，tree height即call stack的层数 <=== ？？？

### Java
```java
class Solution {
    public int sumFromLeavesToRoot(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] sum = {0};
        dfs(root, 1, 0, sum);
        return sum[0];
    }
    
    private void dfs(TreeNode node, int multiply, int curSum, int[] sum) {
        curSum += node.val * multiply;
        multiply *= 10;
        
        // if node is a leaf
        if (node.left == null && node.right == null) {
            sum[0] += curSum;
            return;
        }
        
        if (node.left != null) {
            dfs(node.left, multiply, curSum, sum);
        }
        if (node.right != null) {
            dfs(node.right, multiply, curSum, sum);
        }
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
