---
title: "Find 'Good' Internal Nodes"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_good_internal_nodes.html
folder: algorithm
toc: false
---

## Description
有一种二叉树。它的leaf nodes 都有label，要么"good"，要么"bad"；它的internal nodes 的label 都是""。
对于所有的internal nodes，我们做如下的判定（以下都不适用于leaf nodes）：
* 如果left或right child为null，则cur node 为bad
* 如果left或right child都是leaf
  * 如果它们都是good，则cur node 为good
  * 如果它们两个任一个是bad，则cur node 为bad
* 如果left货right child中的任一个是 internal node
  * 如果任一个 internal child node 为bad，则cur node 为bad
  * 必须所有的internal child node以及leaf child node都是good，那么 cur node 才能是good

求：所有的满足以下条件的internal nodes：
* 它自己是 good
* 它的parent 是 bad

### Example
图例：`a(bad)` 意味着a原本是internal node，之后被判定为 bad node。`f:good` 意味着f原本是leaf node，它原本就是 good node
``` 
                   a(bad)
                 /        \
           b(good)         c(bad)
          /      \        /      \
      e(good)  f:good   d:bad  g(bad)
     /     \                        \
 h:good  i:good                    j:good
```
答案应该是 `{'e', 'b'}`

## Solution 1：我自己的解法，实测了一个case，感觉是正确的

### 这题是(1/5/2019)周六mock题目，最后存档也没给答案，给 Beiping 看下这么写是否ok ？？？

从root往下dfs。

### Complexity
* Time: O(n^2)？？？ 因为构建每个list需要n的时间？？？
* Space: O(n)？？？ 每个暂时性的list的空间需求？？？

### Java
代码虽然看起来长，逻辑并不复杂
```java
// helper class
class Node {
    char val;
    Node left, right;
    String label;
    
    public Node(char val) { // internal node
        this.val = val;
        this.left = null;
        this.right = null;
        this.label = "";
    }
    
    public Node(char val, String label) { // left node, label is "good" or "bad"
        this.val = val;
        this.left = null;
        this.right = null;
        this.label = label;
    }
}

public class Solution {
    public List<Node> findGoodSubroots(Node root) {
        List<Node> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        dfs(root, result);
        return result;
    }
    
    private List<Node> dfs(Node cur, List<Node> result) {
        if (cur == null) { // 没有left或right child 的node，也会被归类为 bad node
            return null;
        }
        
        // if cur node is a leaf, then it will have a label of value "good" or "bad",
        // or it will have a label of value ""
        String curLabel = cur.label;
        if (curLabel == "bad") {
            return null; // 返回null即表示cur node 是bad，bad会向上传染
        }
        if (curLabel == "good") {
            return new ArrayList<Node>(); // 当前node是good leaf 时也返回 空list
        }
        
        List<Node> goodSubrootsBeneathAndIncludingCurNode = new ArrayList<>();
        
        List<Node> leftReturn = dfs(cur.left, result);
        List<Node> rightReturn = dfs(cur.right, result);
        
        // 如果left和right child都是good的，则cur node也是good的
        // 如果left和right child都是bad的，则cur node也是bad的
        // 如果left或right child里的一个是bad，另一个不是bad，则另一个就是我们要求的good subroot，
        // 而同时cur node 也还是被传染成bad 的了
        if (leftReturn != null && rightReturn != null) { // left good, right good
            goodSubrootsBeneathAndIncludingCurNode.addAll(leftReturn);
            goodSubrootsBeneathAndIncludingCurNode.addAll(rightReturn);
            goodSubrootsBeneathAndIncludingCurNode.add(cur);
            return goodSubrootsBeneathAndIncludingCurNode;
        } else if (leftReturn != null) { // left good, right bad
            result.addAll(leftReturn);
            return null; // cur node is bad
        } else if (rightReturn != null) { // left bad, right good
            result.addAll(rightReturn);
            return null; // cur node is bad
        } else { // left bad, right bad
            return null;
        }
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {

        Node a = new Node('a');
        Node b = new Node('b');
        Node c = new Node('c');
        Node d = new Node('d', "bad");
        Node e = new Node('e');
        Node f = new Node('f', "good");
        Node g = new Node('g');
        Node h = new Node('h', "good");
        Node i = new Node('i', "good");
        Node j = new Node('j', "good");
        
        a.left = b;
        a.right = c;
        b.left = e;
        b.right = f;
        c.left = d;
        c.right = g;
        e.left = h;
        e.right = i;
        g.right = j;
        
        Solution solu = new Solution();
        List<Node> result = solu.findGoodSubroots(a); // 'e' and 'b'
    }
}
```

## Reference
* 网上没找到这题

{% include links.html %}
