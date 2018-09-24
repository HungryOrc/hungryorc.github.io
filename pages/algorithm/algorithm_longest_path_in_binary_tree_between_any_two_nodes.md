---
title: "Longest Path in Binary Tree between any Two Nodes (Diameter of Binary Tree)"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longest_path_in_binary_tree_between_any_two_nodes.html
folder: algorithm
toc: false
---

## Description
Given a binary tree, you need to compute the length of the "diameter" of the tree. 
The diameter of a binary tree is the length of the longest path between ANY two nodes in a tree. 
This path may or may not pass through the root.

注意！这一题的要求里，如果最长的path上一共连了6个nodes，则这个最长path的长度只能算是5！不是6！要特别注意不同出题人的不同要求，同样这个题，
其他出题人可能就认为path是6. 另外这一题不care是否是binary search tree，也不care nodes的val是否可以重复。

### Example
* Input:
  ```
      1
     / \
    2   3
   / \     
  4   5 
  ```
  * Output: 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
* Input:
  ```
      1
     /
    2
   /     
  4 
  ```
  * Output: 2, which is the length of the path [4,2,1].
* Input:
  ```
        1
       /
      2
     / \    
    4   5
   /     \
  5       3
  ```
  * Output: 4, which is the length of the path [5,4,2,5,3].  
  
## Solution 1，最符合直觉的方法，但速度很慢

思路要点：
* 以某个node n1 为root来看，包含它和它的所有子树在内，最长的path必然是下面3个值中的一个：
  * n1的左子树里（不通过n1）的最长path
  * n1的右子树里（不通过n1）的最长path
  * 通过n1，也通过n1的左子树以及右子树的最长path，这个必然等于 **左子树的height + 1 + 右子树的height**
* 注意！不允许两次走一条边！所以不能说从n1的左子树上来经过n1再下来回到n1的左子树
* 把求最长path和求height弄成两个函数。这样是最符合直觉的。但很慢。因为向下递归了两轮，大量的浪费
  
### Complexity
* Time: O(n)
* Space: O(height of tree)，call stack的层数等于树高，每层call stack用O(1)的空间

### Java
```java
class Solution {

    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
    
        int diameterOfCurNode = Math.max(
            height(root.left) + height(root.right),
            Math.max(diameterOfBinaryTree(root.left), 
                     diameterOfBinaryTree(root.right))
        );
        
        return diameterOfCurNode;
    }
    
    private int height(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        return 1 + Math.max(height(node.left), height(node.right));
    }
}
```

## Solution 2，把更新maxPathLen和getHeight弄成一个函数了！非常巧妙
这个Solution看起来会像是前一个Solution的改良，但其实并不是！

这个Solution基于一个脱胎换骨的新逻辑：**一个tree里的max path len，等于这个tree里所有nodes分别作为root，每个root的左height加上右height的合，哪一个合最大，即是整个tree的max path len**！
  
### Complexity
* Time: O(n)
* Space: O(height of tree)，call stack的层数等于树高，每层call stack用O(1)的空间

### Java
```java
class Solution {
    int maxPathLen = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
    
        updateMaxPathAndReturnHeight(root);
        
        return maxPathLen;
    }
    
    private int updateMaxPathAndReturnHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        int heightL = updateMaxPathAndReturnHeight(node.left);
        int heightR = updateMaxPathAndReturnHeight(node.right);
        
        // update max path len
        // 注意因为拥有5个nodes的path的len只能算4，所以下面的 heightL + heightR 就
        // 没有再 +1
        maxPathLen = Math.max(maxPathLen, heightL + heightR);
        
        // return height of cur node
        return 1 + Math.max(heightL, heightR);
    }
}
```

## Reference
* [Diameter of Binary Tree [LeetCode]](https://leetcode.com/problems/diameter-of-binary-tree/description/)

{% include links.html %}
