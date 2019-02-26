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

## Solution：Java PreOrder + Queue，速度前 30%
那些适用于所有Binary Tree的 serialize/deserialze 的方法，自然也适用于BST。但如果利用上BST的左小右大的属性，会更快，比如下面这个方法：

Ref: https://leetcode.com/problems/serialize-and-deserialize-bst/

My solution is pretty straightforward and easy to understand, not that efficient though. And the serialized tree is compact.
Pre order traversal of BST will output root node first, then left children, then right.
Generate the string in this way.

To deserialized it, use a queue to recursively get root node, left subtree and right subtree.

### Complexity
* Time: O(n)
* Space: Serialize: O(logn), Deserialize: O(n)

### Java
```java
class Codec {
    private static final String SEPARATOR = ",";
    
    // Encodes a tree to a single string
    // 这个serialize看起来没有直接利用BST左小右大的特点，但内核里其实是用了。
    // 伏笔在于：pre order 一个BST之后，root之后先会有一串比root小的，再会有一串比root大的，
    // 所以这个方法不需要把null nodes 用 "#" 来表示，这就是高效之处。
    // 后面的deserialize也是秉承这个思想做的，二者的内核必须一致。
    public String serialize(TreeNode root) {
        if (root == null) return "";
        
        StringBuilder sb = new StringBuilder();
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            root = stack.pop();
            sb.append(root.val).append(SEPARATOR);
            
            if (root.right != null) {
                stack.push(root.right);
            }
            if (root.left != null) { // 左要放在栈顶，因为之后要先处理它
                stack.push(root.left);
            }
        }
        return sb.toString();
    }

    // Decodes the encoded string to a tree
    // (pre-order traversal)
    public TreeNode deserialize(String data) {
        if (data.equals("")) return null;
        
        String[] strs = data.split(SEPARATOR);
        Queue<Integer> queue = new LinkedList<>();
        
        for (String s : strs) {
            queue.offer(Integer.parseInt(s));
        }
        
        return getNode(queue);
    }

    private TreeNode getNode(Queue<Integer> queue) {
        if (queue.isEmpty()) return null;
        
        TreeNode root = new TreeNode(queue.poll());
        
        // 这里利用了BST的root左边都比root小的特性
        Queue<Integer> samllerQueue = new LinkedList<>();
        while (!queue.isEmpty() && queue.peek() < root.val) {
            samllerQueue.offer(queue.poll());
        }

        root.left = getNode(samllerQueue); // recursion

        // 目前queue里只剩下比root大的元素了
        root.right = getNode(queue);
        
        return root;
    }
}
```

## Reference
* [Serialize and Deserialize BST [LeetCode]](https://leetcode.com/problems/serialize-and-deserialize-bst/description/)

{% include links.html %}
