---
title: "Binary Tree Traversal Upwards by Tier"
tags: [algorithm, binary_tree, binary_tree_traversal, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_upwards_by_tier.html
folder: algorithm
toc: false
---

## Description
逐步给出 binary tree 的各个 leaf node，从下到上，从左到右。要求按照 Tier 输出结果，而非按照 Level 输出结果！这两种方式的区别如下：

### Example
```
      A
    /   \
   B     C
       /   \
      D     E
     / \
    F   G
```
都是从下到上，从左到右的遍历：
* 如果按照 Level，则结果为：[[F,G], [D,E], [B,C], A]
* 如果按照 Tier，则结果为：[[B,F,G,E], D, C, A]
  * B,F,G,E 都是 leaf，它们属于同一个“阶层” (tier)。
  * 如果把上面这几个 leaves 都去掉，那么剩下的树，leaf tier 就是 D。C 和 D 不是一个阶层，C 是更高一个阶层的。去掉 D 以后，C 才变成 leaf。


  
## Solution
Recursively traverse the binary tree, 从上到下。最后的答案要求是从下到上，但我们一开始的思路和步骤是从上到下，层层向下转包。核心思想是：
```java
int tierOfCurNode = Math.max(getTier(node.left), getTier(node.right)) + 1;
```

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class TreeNode {
    char val;
    TreeNode left, right;
}
 
public class Solution {
	
    public List<List<TreeNode>> outputLeaves (TreeNode root) {
        if (root == null) {
            return null;
        }

        List<List<TreeNode>> result = new ArrayList<>();		
        getTier(root, result);
        return result;
    }

    private int getTier(TreeNode node, List<List<TreeNode>> result) {
	// let all leaf nodes to be at Tier 0,
	// then the null nodes one tier below the leaf nodes will be at Tier -1
        if (node == null) {
            return -1;
        }

        int tierOfCurNode = Math.max(getTier(node.left), getTier(node.right)) + 1;

        // result 里最先放入的是最初的那一层 leaves
	// 每加一层，都需要在 result 里先加入一个空的 ArrayList。包括我们第一次放进去最初的那一层leaves的时候
        if (result.size < tierOfCurNode + 1) {
            result.add(new ArrayList<TreeNode>());
        }

        result.get(tierOfCurNode).add(curNode);

        return tierOfCurNode;
    }
}
```

{% include links.html %}
