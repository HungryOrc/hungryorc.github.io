---
title: "Convert Ternary Expression to Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_ternaryExpression_toBinaryTree.html
folder: algorithm
toc: false
---

## Description
问号后面是左子树，冒号后面是右子树。给的expression string 保证是 valid的

Convert a ternary expression to a binary tree.
Assume that all these ternary expression are valid, 
connamely they won't cause any error or conflict when building the binary trees accordingly.
```java
class TreeNode {
    String val;
    TreeNode left, right;
    public TreeNode(String v) {
        this.val = v;
    }
}
```

### Example
* Input: `a ? b ? c : d : e`，其实就是 `a ? (b ? c : d) : e`
  * Output: 
    ```
        a
       / \
      b   e
     / \
    c   d
    ```
* Input: `a ? b : c ? d : e`，其实就是 `a ? b : (c ? d : e)`
  * Output: 
    ```
      a
     / \
    b   c
       / \
      d   e 
    ```

## Solution
1. 这是一个valid的ternary string，所以一个root 如果拥有 "?分句"即左子树，它一定同时也会拥有 “:分句”即右子树。一个root如果没有左子树那么也一定没有右子树
2. 一个root不可能只有右子树而没有左子树
3. 因为expression 是 valid的，所以 **问号后面的第一个char 一定是左子树的root node！冒号后面的第一个char 一定是右子树的root node！**
4. 一个root的左子树可能非常巨大，但是都处理完了以后，接下来的第一个node一定是它的右子树的sub root。同时它的右子树也有可能很巨大

### Complexity
* Time: O(n)
* Space: O(tree height), 即 call stack's depth

### Java
```java
public class Solution {
    public TreeNode ternaryToTree(String ternary) {
        if (ternary == null || ternary.length() == 0) {
            return null;
        }
        
        String[] strs = ternary.split(" ");
        return dfs(strs, new int[1]);
    }
    
    private TreeNode dfs(String[] strs, int[] curIndex) {
        String s = strs[curIndex[0]];
        TreeNode root = new TreeNode(s);
        
        curIndex[0]++;
        
        // 下面if里的后半句是全题的关键！即：
        // cur string等于问号的时候，才设置当前root的左右child node，
        // 否则cur string等于冒号的时候，就直接什么都不做了！
        if (curIndex[0] < strs.length && strs[curIndex[0]].equals("?")) {
            curIndex[0]++;
            root.left = dfs(strs, curIndex);
            
            curIndex[0]++;
            root.right = dfs(strs, curIndex);            
        }
        return root;
    }
}
```

## Reference
在网上没找到这题。只有leetcode上有一个关于这题的讨论话题，但没有这题可以做

{% include links.html %}
