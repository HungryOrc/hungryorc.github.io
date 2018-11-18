---
title: "Serialize and Deserialize N-ary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_serialize_and_deserialize_n_ary_tree.html
folder: algorithm
toc: false
---

## Description
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following `3-ary` tree:
```
         1
      /  |  \
     3   2   4
    / \
   5   6
```
as `[1[3[5 6] 2 4]]`, or what ever other kinds of format, you do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

Notice:
* `N` is in the range of `[1, 1000]`
* Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

The class of the Nodes in the N-ary Trees is:
```java
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
}
```

Your `Codec` object will be instantiated and called as such:
```java
Codec codec = new Codec();
codec.deserialize(codec.serialize(root));
```

### Example
略

## Solution 1，LeetCode的方法，速度快，把树弄成 1,3,3,2,5,0,6,0,2,0,4,0
Ref: https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/discuss/151421/Java-preorder-recursive-solution-using-queue

对于原题中举例的那个树，serialize成这样：`1,3,3,2,5,0,6,0,2,0,4,0`。Recursion using Queue.

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(height of tree), 即call stack 的层数 <=== 对么 ？？？

### Java
```java
class Codec {
    private static final String SPLITTER = ",";
    
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if (root == null) {
            return "";
        }
        
        List<String> list = new ArrayList<>();
        dfsSer(root, list);
        
        return String.join(SPLITTER, list);
    }
    
    private void dfsSer(Node root, List<String> list) {
        list.add(String.valueOf(root.val));
        list.add(String.valueOf(root.children.size()));
        
        for (Node child : root.children) {
            dfsSer(child, list);
        }
    }

    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data.isEmpty()) {
            return null;
        }
        
        Queue<String> queue = new LinkedList<>();
        String[] strings = data.split(",");
        for (String s : strings) {
            queue.offer(s);
        }
        
        return dfsDeser(queue);
    }
    
    private Node dfsDeser(Queue<String> queue) {
        int val = Integer.parseInt(queue.poll());
        int size = Integer.parseInt(queue.poll());
        
        Node root = new Node(val, new ArrayList<Node>());
        
        // 整个方法的灵魂在这一个 for loop！
        for (int i = 0; i < size; i++) {
            root.children.add(dfsDeser(queue));
        }
        
        return root;
    }
}
```

## Solution 2，我自己的方法，速度慢，把树弄成 [1[3[5 6] 2 4]]
对于原题中举例的那个树，serialize成这样：`[1[3[5 6] 2 4]]`，对于每个node，它的所有children放在一对`[...]`里面，`[`紧贴着parent node的val，`[...]`里面的相邻child之间用一个空格 ' ' 分开。

### Complexity
* Time: O(n * tree height) <=== 对么？？？
* Space: O(n * tree height) <=== 对么？？？

### Java
```java
class Codec {
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if (root == null) {
            return "";
        }
        
        StringBuilder sb = new StringBuilder();
        sb.append('[');
        dfsSer(root, sb);
        sb.append(']');

        return sb.toString();
    }
    
    private void dfsSer(Node root, StringBuilder sb) {
        sb.append(root.val);
        
        if (root.children.size() > 0) {
            sb.append('[');
            for (Node child : root.children) {
                dfsSer(child, sb);
                sb.append(' ');
            }
            // remove the last space
            sb.deleteCharAt(sb.length() - 1);
            sb.append(']');
        }
    }

    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data == "") {
            return null;
        }
        
        // remove the out most '[' and ']' by setting
        // the start index to be 1 and end index to be n-2
        return dfsDeser(data, 1, data.length() - 2);
    }
    
    private Node dfsDeser(String data, int start, int end) {
        System.out.println(start + ", " + end);
        
        int[] result = getVal(data, start);
        int val = result[0];
        start = result[1];
        Node cur = new Node(val, new ArrayList<Node>());
        
        // remove the val and the out most '[' and ']', if any
        start += 2;
        end--;
        if (start <= end) {
            List<int[]> ranges = getChildrensRanges(data, start, end);
            
            for (int[] range : ranges) {
                cur.children.add(dfsDeser(data, range[0], range[1]));
            }
        }
        return cur;
    }
    
    private List<int[]> getChildrensRanges(String data, int start, int end) {
        List<int[]> ranges = new ArrayList<>();
        
        int count = 0;
        for (int i = start; i <= end; i++) {
            if (data.charAt(i) == '[') {
                count++;
            } else if (data.charAt(i) == ']') {
                count--;
            }
            
            if (count == 0 && data.charAt(i) == ' ') {
                System.out.println("range: " + start + ", " + (i - 1));
                ranges.add(new int[]{start, i - 1});
                start = i + 1;
            }
        }
        
        // for the last node before the final ']'
        ranges.add(new int[]{start, end});
        return ranges;
    }
    
    private int[] getVal(String data, int start) {
        int val = 0;
        while (isDigit(data.charAt(start))) {
            val *= 10;
            val += data.charAt(start) - '0';
            start++;
        }
        
        int[] result = new int[2];
        result[0] = val;
        result[1] = start - 1;
        
        return result;
    }
    
    private boolean isDigit(char c) {
        return c - '0' >= 0 && c - '0' <= 9;
    }
}
```

## Reference
* [Serialize and Deserialize N-ary Tree [LeetCode]](https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/description/)

{% include links.html %}
