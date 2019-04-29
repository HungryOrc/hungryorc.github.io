---
title: "Get Tree Nodes in BST Whose Keys are within Certain Range"
tags: [algorithm, binary_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_keys_in_certain_range_in_binary_search_tree.html
folder: algorithm
toc: false
---

## Description
Get the list of keys in a given binary search tree in a given range[min, max] in ascending order, both min and max are inclusive.

If there are no keys in the given range, return an empty list.

```java
public class TreeNode {
    int key;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
略

## Solution: Recursion
对于BST，我们知道通过 in-order traversal 可以获得所有节点从小到大的排布。那我们就从这里入手，
做一个 “每一步都经过判断的 in-order traversal”：
* 只有当 root.val > lowerBoundOfRange 的时候，我们才有必要 深入左子树
* 只有当 root.val < upperBoundOfRange 的时候，我们才有必要 深入右子树
* 深入任何子树都有可能最后是一无所获

### Complexity
* Time: O(logn + |k2 - k1|)
  * 注意！完全有可能某一个方向沿着root到leaf一路走到底，都没有找到一个符合range的node，但按照本方法，还就是需要一直找到leaf
  * 这里的处理逻辑，其实可以认为是由2部分构成的：
    * 在 BST 里查找 k1 和 k2
    * 遍历 k2 - k1 的部分
  * 第一部分和第二部分的开销，在不同情况下，不一定谁dominate。
    * 比如刚好 k2 - k1 的部分没有任何一个node，这个时候前面的logn会dominate
    * 如果刚好k2 - k1的部分包含了所有的node，那么这部分会dominate
    * 所以正确的复杂度是 logn + |k2 - k1|
* Space: O(logn), call stack的层数

### Java
```java
public class Solution {
  public List<Integer> getRange(TreeNode root, int min, int max) {
    List<Integer> result = new ArrayList<Integer>();
    
    rangedInorderTraversal(root, min, max, result);
    return result;
  }
  
  // overload
  private void rangedInorderTraversal(TreeNode node, int min, int max, List<Integer> result) {
    if (node == null) {
      return;
    }
    
    // 特别注意！下面的3个if之间不是互斥关系！而是顺序关系！它们三个都要做的！
    if (node.key > min) {
      rangedInorderTraversal(node.left, min, max, result);
    }
    if (node.key >= min && node.key <= max) {
      result.add(node.key);
    }
    if (node.key < max) {
      rangedInorderTraversal(node.right, min, max, result);
    }
  }
}
```

## Reference
* [Get Keys in Binary Search Tree in Given Range [LaiCode]](https://app.laicode.io/app/problem/55)

{% include links.html %}
