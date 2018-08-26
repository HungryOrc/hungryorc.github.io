---
title: "Binary Tree Traversal Upwards by Tier"
tags: [algorithm, binary_tree, binary_tree_traversal]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_upwards_by_tier.html
folder: algorithm
toc: false
---

## Description
逐步给出 binary tree 的各个 leaf node，从下到上，从左到右。要求按照 Tier 输出结果，而非按照 Level 输出结果！这两种方式的区别如下：

## Example
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

## Complexity
* Time: O(n)
* Space: O(n)
  
## Solution
Recursive way.
### Java
```java
class TreeNode {
	char val;
	TreeNode left;
	TreeNode right;
}

// 
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

if (result.size < tierOfCurNode + 1) {
	result.add(new ArrayList<TreeNode>());
}

result.get(tierOfCurNode).add(curNode);

return tierOfCurNode;
}
}
```


{% include links.html %}
