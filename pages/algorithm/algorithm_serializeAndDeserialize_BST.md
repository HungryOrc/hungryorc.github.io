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

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be **stateless**.

### Example
无法举例

## Solution

### Complexity
* Time: O(n)
* Space: Serialize: O(logn), Deserialize: O(n)

### Java
```java
class Solution {
    // Serialize
    public String serialize(TreeNode root) {
        if (root == null) {
            return "{}";
        }

        ArrayList<TreeNode> queue = new ArrayList<TreeNode>();
        queue.add(root);

        // 把所有的左右children全部加上，不管这里面有没有null node
        // 注意！这里的queue size是不断增加的！！
        // 这里用while似乎更符合直觉，但其实更繁琐，还不如用for，习惯以后更好
        for (int i = 0; i < queue.size(); i++) {
            TreeNode node = queue.get(i);
            if (node == null) {
                continue;
            }
            queue.add(node.left);
            queue.add(node.right);
        }

        // 把queue的结尾的那些null都去掉，一直到结尾不是null
        while (queue.get(queue.size() - 1) == null) {
            queue.remove(queue.size() - 1);
        }

        StringBuilder sb = new StringBuilder();
        sb.append("{");
        sb.append(queue.get(0).val);
        for (int i = 1; i < queue.size(); i++) {
            if (queue.get(i) == null) {
                sb.append(",#");
            } else {
                sb.append(",");
                sb.append(queue.get(i).val);
            }
        }
        sb.append("}");
        return sb.toString();
    }
    
    // Deserialize
    public TreeNode deserialize(String data) {
        if (data.equals("{}")) {
            return null;
        }
        
        // substring 函数的第一个参数是 inclusive 的，第二个参数是 exclusive 的
        // 所以下面这样写，就是把 String data 里的第一个和最后一个字符都去掉了，即花括号
        String[] vals = data.substring(1, data.length() - 1).split(",");
        ArrayList<TreeNode> queue = new ArrayList<TreeNode>();
        
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        queue.add(root);
        
        int index = 0; // 这个参数指示：当前正在处理String数组里的第几个String
        boolean isLeftChild = true;
        
        // 注意！！index 和 i 不是一回事！！！
        // index表示当前的 parent node 是哪个，i 表示当前在查看第几个node的值
        for (int i = 1; i < vals.length; i++) {
            
            // 如果第i个String不表示null node，就做以下的事。如果是null node，就什么也不做
            if (!vals[i].equals("#")) {
                TreeNode node = new TreeNode(Integer.parseInt(vals[i]));
                if (isLeftChild) {
                    queue.get(index).left = node;
                } else {
                    queue.get(index).right = node;
                }
                queue.add(node);
            }
            
            // 当这个node的左右子节点都搞定了，就可以处理下一个node了
            if (!isLeftChild) {
                index++;
            }
            isLeftChild = !isLeftChild;
        }
        return root;
    }
}
```

## Reference
* [Serialize and Deserialize BST [LeetCode]](https://leetcode.com/problems/serialize-and-deserialize-bst/description/)

{% include links.html %}
