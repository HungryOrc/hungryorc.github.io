---
title: "Serialize and Deserialize (Ordinary) Binary Tree (Not BST)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_serializeAndDeserialize_BinaryTree.html
folder: algorithm
toc: false
---

## Description
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure. **The encoded string should be as compact as possible.**
```java
 class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```
Your Codec object will be instantiated and called as such:
```java
Codec codec = new Codec();
codec.deserialize(codec.serialize(root));
```

Note:
* Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be **stateless**.
* Values (int) of TreeNodes can be more than one digit.

这题被LC标注为Hard题，但其实只是 median 难度。**这一题 特别 特别 热门！很多公司都喜欢考！**

这题的解法适用于任何binary tree，自然也适用于BST。不过对于BST，如果利用它的左小右大的属性，可以有更高效的serialize/deserialize的方法

### Example
略

## Solution 1: Pre-order 来进行 serialize 和 deserialize。这种做法对tree来说是最简单最直观的，比level order 还简明易写

### Complexity
* Time: O(n)
* Space: Serialize: O(height of tree), Deserialize: O(n)

### Java
```java
public class Solution {
    private static final String SPLITTER = ",";
    private static final String NN = "#";
    
    // encode a binary tree to a string
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        buildString(root, sb);
        return sb.toString();
    }
    
    private boid buildString(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append(NN).append(SPLITTER);
            return;
        }
        
        sb.append(node.val).append(SPLITTER);
        
        buildString(node.left, sb);
        buildString(node.right, sb);
        
        // 这样做的话最后会有一串 null 的字符，即一串 ",#,#,#,#..."，留着的话也不会导致错误，
        // 就是说在后面deserialize 的时候，末尾的这些# 也会被正确地处理的
        // 如果想在这里就去掉这些没用的尾巴的话，也是一种优化。用一个while loop 就行了
    }
    
    // decode the string back to tree, return the root node
    public TreeNode deserialize(String str) {
        String[] arr = str.split(SPLITTER);
        
        // 为了可以从前面把数拿走！
        Deque<String> preorder = new ArrayDeque<>();
        preorder.addAll(arr);
        return reconstruct(preorder);
    }
    
    private TreeNode reconstruct(Deque<String> preorder) {
        String cur = preorder.pollFirst();
        
        // base case
        if (cur.equals(NN)) return null;
        
        TreeNode root = new TreeNode(Integer.valueOf(cur));
        root.left = reconstruct(preorder);
        root.right = reconstruct(preorder);
        return root;
    }
}
```

## Solution 2：Level Order Traversal

### Complexity
* Time: O(n)
* Space: Serialize: O(height of tree), Deserialize: O(n)

### Java
```java
class Solution {
    private static final String SPLITTER = ",";
    private static final String NN = "#";

    // Serialize a tree to a string
    public String serialize(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        StringBuilder sb = new StringBuilder();
        
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            
            if (cur == null) {
                sb.append(NN).append(SPLITTER);
                continue;
            }
            
            sb.append(cur.val).append(SPLITTER);

            queue.offer(cur.left); // 是null也放进去
            queue.offer(cur.right); // 是null也放进去
        }
        return sb.toString();
    }
    
    // Deserialize string back to tree, return the root node
    public TreeNode deserialize(String data) {
        String[] vals = data.split(SPLITTER);
        if (vals[0].equals(NN)) return null;
        
        Queue<TreeNode> queue = new LinkedList<>();
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        queue.offer(root);
  
        for (int i = 1; i < vals.length; i++) {
            TreeNode parent = queue.poll();
            
            // generate 2 children
            // 如果当前value不是#，那么就把它弄成当前parent的left child！
            // 如果是#，就不管它，那么当前parent的left child自然而然就会是 null。
            // 最后，不管是不是#，后面都要继续下去！因为还要看当前parent的right child是怎么回事，它是不是null，
            // 以下以此类推
            if (!vals[i].equals(NN)) {
                parent.left = new TreeNode(Integer.valueOf(vals[i]));
                queue.offer(parent.left);
            }
            
            i++;
            if (!vals[i].equals(NN)) {
                parent.right = new TreeNode(Integer.valueOf(vals[i]));
                queue.offer(parent.right);
            }
        }
        return root;
    }
}
```

## Reference
* [Serialize and Deserialize Binary Tree [LeetCode]](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)

{% include links.html %}
