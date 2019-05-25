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
* Space: O(n)

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
