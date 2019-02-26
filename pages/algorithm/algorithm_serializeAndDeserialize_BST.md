---
title: "Serialize and Deserialize BST (Not Ordinary Binary Tree)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_serializeAndDeserialize_BST.html
folder: algorithm
toc: false
---

## Description
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
```java
 class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```
Design an algorithm to serialize and deserialize a **Binary Search Tree (BST)。如果是一般的Binary Tree，会更难做**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

Your Codec object will be instantiated and called as such:
```java
Codec codec = new Codec();
codec.deserialize(codec.serialize(root));
```
**The encoded string should be as compact as possible.**

Note:
* Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be **stateless**.
* Values (int) of TreeNodes can be more than one digit.

### Example
无法举例

## Solution：Java PreOrder + Queue
那些适用于所有Binary Tree的 serialize/deserialze 的方法，自然也适用于BST。但如果利用上BST的左小右大的属性，会更快，比如下面这个方法：

Ref: https://leetcode.com/problems/serialize-and-deserialize-bst/

My solution is pretty straightforward and easy to understand, not that efficient though. And the serialized tree is compact.
Pre order traversal of BST will output root node first, then left children, then right.
Pre order traversal is BST's serialized string. 

To deserialized it, use a queue to recursively get root node, left subtree and right subtree.

I think time complexity is O(NlogN), worst case complexity should be O(N^2), when the tree is really unbalanced.

### Complexity
* Time: O(n)
* Space: Serialize: O(logn), Deserialize: O(n)

### Java
```java
public class Codec {
    private static final String SEP = ",";
    private static final String NULL = "null";
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        if (root == null) return NULL;
        Stack<TreeNode> st = new Stack<>();
        st.push(root);
        while (!st.empty()) {
            root = st.pop();
            sb.append(root.val).append(SEP);
            if (root.right != null) st.push(root.right);
            if (root.left != null) st.push(root.left);
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    // pre-order traversal
    public TreeNode deserialize(String data) {
        if (data.equals(NULL)) return null;
        String[] strs = data.split(SEP);
        Queue<Integer> q = new LinkedList<>();
        for (String e : strs) {
            q.offer(Integer.parseInt(e));
        }
        return getNode(q);
    }
    
    // some notes:
    //   5
    //  3 6
    // 2   7
    private TreeNode getNode(Queue<Integer> q) { //q: 5,3,2,6,7
        if (q.isEmpty()) return null;
        TreeNode root = new TreeNode(q.poll());//root (5)
        Queue<Integer> samllerQueue = new LinkedList<>();
        while (!q.isEmpty() && q.peek() < root.val) {
            samllerQueue.offer(q.poll());
        }
        //smallerQueue : 3,2   storing elements smaller than 5 (root)
        root.left = getNode(samllerQueue);
        //q: 6,7   storing elements bigger than 5 (root)
        // 目前q里只剩下比root大的元素了
        root.right = getNode(q);
        return root;
    }
}
```

## Reference
* [Serialize and Deserialize BST [LeetCode]](https://leetcode.com/problems/serialize-and-deserialize-bst/description/)

{% include links.html %}
