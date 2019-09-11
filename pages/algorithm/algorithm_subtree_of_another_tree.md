---
title: "Subtree of Another Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_subtree_of_another_tree.html
folder: algorithm
toc: false
---

## Description
Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.
```java
// Definition for a binary tree node.
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```
这题在LC里被标为 easy 题，但我感觉至少是 median 题

### Example
* Input: 
  * tree s
    ```
        3
       / \
      4   5
     / \
    1   2
    ```
  * tree t
    ```
      4 
     / \
    1   2
    ```
  * Output: True, because t has the same structure and node values with a subtree of s.
* Input: 
  * tree s
    ```
        3
       / \
      4   5
     / \
    1   2
       /
      0
    ```
  * tree t
    ```
      4 
     / \
    1   2
    ```
  * Output: False, 因为这题要求，t必须是s的一个子树，且t的每一个leaf node 都必须是s里的 leaf node！即t不能是s里的中间的某一段！t里的每个底部node都必须也是s里的底部node

## Solution：Tree的Serialize和Deserialize，用 ”in-order traversal“ 和 ”补足所有的null node“ 这两方面结合起来。速度 前5%
如果仅用 in-order traversal，是不能唯一地确定一个 tree 的！但**如果把leaf node 下面的所有 null nodes 都表达出来（比如都用一个 # 符号），那么这样做出来的 in-order traversal 是可以唯一地确定一个树的！**

注意：**node里的value可能有多位数，还可能有负号**

### Complexity
* Time: O(ns + nt)，ns是tree s 里的nodes个数，nt是tree t 里的nodes个数 <==== ？？？？
* Space: O(max(ns, nt))，因为我们做recursion的时候有 call stack <==== ？？？？

### Java
```java
class Solution {
    public static final String SPLITTER = ",";
    public static final String NULL = "#";

    public boolean isSubtree(TreeNode s, TreeNode t) {
        if (s == null && t == null) return true;
        if (s == null || t == null) return false;
        
        StringBuilder sbS = new StringBuilder();
        sbS.append(SPLITTER);
        serializeTree(s, sbS);
        String serS = sbS.toString();
        
        StringBuilder sbT = new StringBuilder();
        sbT.append(SPLITTER);
        serializeTree(t, sbT);
        String serT = sbT.toString();
        
        return serS.contains(sbT);
    }
    
    private void serializeTree(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append(NULL).append(SPLITTER);
            return;
        }
        
        sb.append(node.val).append(SPLITTER);
        
        serializeTree(node.left, sb);
        serializeTree(node.right, sb);
    }
}
```

## Reference
* [Subtree of Another Tree [LeetCode]](https://leetcode.com/problems/subtree-of-another-tree/description/)

{% include links.html %}
