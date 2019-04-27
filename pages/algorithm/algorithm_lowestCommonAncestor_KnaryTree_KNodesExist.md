---
title: "Lowest Common Ancestor in K-nary Tree for K Nodes that Exist"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_lowestCommonAncestor_KnaryTree_KNodesExist.html
folder: algorithm
toc: false
---

## Description
参考
* 在 binary tree 里找 K个一定存在的Nodes 的LCA 的方法，以及
* 在 k-nary tree 里找 2个一定存在的Nodes 的LCA 的方法

Notice: Assume these K nodes do exist in this tree.

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
    public TreeNode LCA_KNodes_KBranchTree(TreeNode root, Set<TreeNode> targets) {
        if (root == nul || targets == null || targets.size() == 0) {
            return null;
        }
        if (targets.contains(root)) { // 有意思！不用再往下看了！
            return root;
        }
    
        // 注意！这个计数不是global的，而只是这一层的。这个计数不会往上一层再传递。
        // 但在上一层的处理里，每一个不为null的child，自然会再次给上一层那里的计数+1，
        // 所以，每一层的计数，它的肉体并不会升华，但它的精神会永远一代代被继承下去！兽人永不为奴！
        int numOfNodesFound = 0;
        TreeNode tmpResult = null;
        
        for (TreeNode child : root.children) {
            TreeNode resultFromThisChild = LCA_KNodes_KBranchTree(child, one, two);
            
            if (resultFromThisChild != null) {
                numOfNodesFound ++;
            
                // 精华在这一句
                // 如果有所斩获的child 的个数 >= 2个，则表示当前root一定是 某个层次上的 小头目，它的下面
                // 一定有 >= 2个 我们要找的 target nodes，但到底是几个，我们不用关心了。因为k个nodes都必存在
                // 到了这里，我们已经可以在这个层次上放心大胆地返回这个root了 ！！！ 其他的都不用care了 ！！！ 哦也 ！！！
                if (numOfNodesFound > 1) {
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
