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
哦也

### Example
* Input: 
  * Output: 

## Solution 2，我自己的方法，速度慢，把树弄成这种样子 [1[3[5 6] 2 4]]
对于原题中举例的那个树，serialize成这样：`[1[3[5 6] 2 4]]`，对于每个node，它的所有children放在一对`[...]`里面，`[`紧贴着parent node的val，`[...]`里面的相邻child之间用一个空格 ` ` 分开。

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
