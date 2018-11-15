---
title: "Identical Binary Trees after Tweaking Left and Right Subtrees"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_identical_binary_trees_tweaking_left_and_right_subtrees.html
folder: algorithm
toc: false
---

## Description
如果两个树，能通过任意多次地tweak它们里面的任意nodes，实现两树的完全相同（所有位置所有值都相同），那么这两个树之间的关系就叫做 "tweaked identical"

Determine whether two given binary trees are identical assuming any number of ‘tweak’s are allowed. 
A "tweak" is defined as a swap of the children of **any** node in the tree.
```java
public class TreeNode {
    int key;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { key = x; }
}
```

### Example
* Input: Given the following 2 binary trees:
  ```
        5                 5
      /   \             /   \
     3     8           8     3
   /   \                   /   \
  1     4                 4     1
  ```
  * Output: they are "tweaked identical"

## Solution 1: Recursion

### Complexity
* Time: O(4^height of tree)，注意这个**4**！因为每个`isTweakedIdentical`都会引发4个分支的`isTweakedIdentical`，所以是4！
  * 如果height of tree是logn（n为treenode的个数），则 Time = O(4^logn) = O(n^2)
* Space: O(height of tree), call stack的层数

### Java
```java
public class Solution {
    public boolean isTweakedIdentical(TreeNode one, TreeNode two) {
        if (one == null && two == null) {
            return true;
        } else if (one == null || two == null) {
            return false;
        }
      
        // one != null && two != null
        if (one.key != two.key) {
            return false;
        }
      
        return (isTweakedIdentical(one.left, two.left) && isTweakedIdentical(one.right, two.right))
            || (isTweakedIdentical(one.left, two.right) && isTweakedIdentical(one.right, two.left));
    }
}
```

## Solution 2: Iteration，试了好久，没能写出来
这个题目不知道是否能用iteration写，因为它的难度不在于有4个分支，而是这4个分支还要不断切换，比如说这一层是扭转左右的，下一层可能又不扭转，再下一层可能又要扭转......

## Reference
* [Tweaked Identical Binary Trees [LaiCode]](https://app.laicode.io/app/problem/50)

{% include links.html %}
