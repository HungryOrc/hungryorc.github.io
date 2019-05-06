---
title: "Number of Uni-Value Trees (All Nodes Have the Same Value)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_number_of_uni_value_trees_all_nodes_have_same_value.html
folder: algorithm
toc: false
---

## Description
A uni-value tree is a tree/subtree with all the nodes (from its root to all its leaves) having the same value. For a given tree, 
count the number of such uni-value subtress in it.

### Example
* Input: 
  ```
       5
     /   \
    5     5
        /   \
       2     1
      /     / \
     2     1   1
  ```
  * Output: 6。其中：4个leaves都算是。然后还有2个nodes：2个2里上面的那个2，以及3个1里上面的那个1

## Solution：DFS Recursion，速度 前1%

### Complexity
* Time: O(n)
* Space: O(tree height)

### Java
```java
class Solution {
    public int countUnivalSubtrees(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] count = {0};
        countUni(root, count);
        return count[0];
    }
    
    private boolean countUni(TreeNode node, int[] count) {
        if (node == null) {
            return true;
        }
        
        boolean left = countUni(node.left, count);
        boolean right = countUni(node.right, count);
        // 先做完left和right，再做下面的判断！不能反过来！这里不是要减支的时候！因为
        // 就算当前node不是univalue subtree的root，那么cur node下面的任何node
        // 都还是可能是univalue subtree的root的，所以左右都要保证往下走下去
        if (!left || !right) {
            return false;
        }
        
        if (node.left != null && node.left.val != node.val) {
            return false;
        }
        if (node.right != null && node.right.val != node.val) {
            return false;
        }
        
        count[0]++;
        return true;   
    }
}
```

## Reference
* [Count Univalue Subtrees [LeetCode]](https://leetcode.com/problems/count-univalue-subtrees/description/)

{% include links.html %}
