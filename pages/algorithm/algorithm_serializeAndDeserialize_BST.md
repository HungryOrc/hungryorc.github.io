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

## Solution：朴素方法

### Complexity
* Time: O(n)
* Space: Serialize: O(logn), Deserialize: O(n)

### Java
```java
class Codec {
    
    // Serialize
    public String serialize(TreeNode root) {
        if (root == null) {
            return "";
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        StringBuilder sb = new StringBuilder();
        
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            
            if (cur == null) {
                sb.append("#,");
                continue;
            }
            
            sb.append(cur.val);
            sb.append(",");
            queue.offer(cur.left); // 是null也放进去
            queue.offer(cur.right); // 是null也放进去
        }

        // 别忘了：把queue的结尾的那些null都去掉，一直到结尾不是null，很重要！
        while (sb.charAt(sb.length() - 1) == '#' || sb.charAt(sb.length() - 1) == ',') {
            sb.deleteCharAt(sb.length() - 1);
        }
        return sb.toString();
    }
    
    // Deserialize
    public TreeNode deserialize(String data) {
        if (data.equals("")) {
            return null;
        }
        
        String[] vals = data.split(",");
        List<TreeNode> list = new ArrayList<>();
        
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        list.add(root);
        
        int index = 0; // 这个参数指示：当前正在处理String数组里的第几个String
        boolean isLeftChild = true;

        // index表示当前的 parent node 是哪个，i 表示当前在查看第几个node的值
        for (int i = 1; i < vals.length; i++) {
            String curVal = vals[i];
            
            // 如果第i个String不表示null node，就做以下的事。如果是null node，就什么也不做
            if (!curVal.equals("#")) {
                TreeNode node = new TreeNode(Integer.parseInt(curVal));
                if (isLeftChild) {
                    list.get(index).left = node; // 注意index一开始是0！
                } else {
                    list.get(index).right = node;
                }
                list.add(node);
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
