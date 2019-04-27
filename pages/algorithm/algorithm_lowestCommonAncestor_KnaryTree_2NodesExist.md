---
title: "Lowest Common Ancestor in K-nary Tree for 2 Nodes that Exist"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_lowestCommonAncestor_KnaryTree_2NodesExist.html
folder: algorithm
toc: false
---

## Description
参考在 binary tree 里找 2个一定存在的Nodes的 最低公共祖先 的方法

Notice: Assume these 2 nodes do exist in this tree.

```java
class TreeNode {
    int val;
    List<TreeNode> children;
    public TreeNode(int x) { 
        val = x; 
        children = new ArrayList<>();
    }
}
```

### Example
无

## Solution：Recursion

### Complexity
* Time: O(tree height)
* Space: O(tree height)，call stack的层数

### Java
```java
public class Solution {
    public TreeNode LCA_2Nodes_KBranchTree(TreeNode root, TreeNode one, TreeNode two) {
        if (root == null) {
            return null;
        }
        if (root == one || root == two) { // 不用再往下看了
            return root;
        }
    
        // 注意！这个计数不是global的，而只是这一层的。这个计数不会往上一层再传递。
        // 但在上一层的处理里，每一个不为null的child，自然会再次给上一层那里的计数+1，
        // 所以，每一层的计数，它的肉体并不会升华，但它的精神会永远一代代被继承下去！兽人永不为奴！
        int numOfNodesFound = 0;
        TreeNode tmpResult = null;
        
        for (TreeNode child : root.children) {
            TreeNode resultFromThisChild = LCA_2Nodes_KBranchTree(child, one, two);
            
            if (resultFromThisChild != null) {
                numOfNodesFound ++;
            
                // 精华在这一句
                // 如果有所斩获的child为2个，则表示当前root一定就是最终的答案。返回它就可以了
                // 当然，这个返回不会完全终止整个程序，而是返回给当前root的parent，然后逐层往上返，一直到整个树的root
                if (numOfNodesFound == 2) {
                    return root;
                } else { // numOfNodesFound == 1
                    tmpResult = resultFromThisChild;
                } 
            }
        }
        return tmpResult;
    }
}
```

## Reference
没找这题

{% include links.html %}
