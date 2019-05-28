---
title: "Serialize and Deserialize N-ary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_serializeAndDeserialize_N-aryTree.html
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

## Solution 1，DFS Recursion，速度前10%，记录各个node的val和children size
Ref: https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/discuss/151421/Java-preorder-recursive-solution-using-queue

对于原题中举例的那个树，serialize成这样：`1,3,3,2,5,0,6,0,2,0,4,0`
* 第一个1是root的val，1后面的3是root的children的size
* 之后的3是root的第一个child的val，然后的2是这个child的children的size
* 再之后的5是root的第一个child的第一个child的val，然后的0是这个grand child的children的size。可见这是一种DFS。之后以此类推

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(height of tree), 即call stack 的层数，但是string array的size又是n的量级，该取哪个？ <=== 对么 ？？？

### Java
```java
class Codec {
    private static final String SPLITTER = ",";
    
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if (root == null) {
            return "";
        }
        
        StringBuilder sb = new StringBuilder();
        dfsSer(root, sb);
        
        return sb.toString();
    }
    
    private void dfsSer(Node root, StringBuilder sb) {
        sb.append(root.val);
        sb.append(SPLITTER);
        sb.append(root.children.size());
        sb.append(SPLITTER);
        
        // 这个方法的灵魂 在这个 for loop 里
        for (Node child : root.children) {
            dfsSer(child, sb);
        }
    }

    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data.equals("")) { // 不要用 data == ""
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
        int childrenSize = Integer.parseInt(queue.poll());
        
        Node root = new Node(val, new ArrayList<Node>());
        
        // 这个方法的灵魂 在这个 for loop 里
        for (int i = 0; i < childrenSize; i++) {
            root.children.add(dfsDeser(queue));
        }
        
        return root;
    }
}
```

## Solution 2，BFS Iteration，速度比上面的DFS稍微慢一点，记录各个node的val和children size
这个方法是我根据上面的leetcode的DFS方法想出来的BFS方法。因为用到多个Queue，速度会比前一个方法慢一些。

对于原题中举例的那个树，serialize成这样：`1,3,3,2,2,0,4,0,5,0,6,0`
* 第一个1是root的val，1后面的3是root的children的size
* 之后的3是root的第一个child的val，然后的2是这个child的children的size。再往后的2是root的第二个child的val，然后的0是这个child的children的size。然后是root的第三个child......
* root的3个children完了以后，再之后的5是root的第一个child的第一个child的val，然后的0是这个grand child的children的size。可见这是一种BFS，一种逐层的遍历。之后以此类推

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(n), string array，以及 几个queue 的size，这个方法没有recursion的call stack  <=== 对么 ？？？

### Java
```java
class Codec {
    private static final String SPLITTER = ",";
    
    // Encodes a tree to a single string.
    public String serialize(Node root) {
        if (root == null) {
            return "";
        }
        
        StringBuilder sb = new StringBuilder();
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        bfsSer(sb, queue);

        return sb.toString();
    }
    
    private void bfsSer(StringBuilder sb, Queue<Node> queue) {
        while(!queue.isEmpty()) { // 不要用 data == ""
            Node cur = queue.poll();
            
            sb.append(cur.val);
            sb.append(SPLITTER);
            sb.append(cur.children.size());
            sb.append(SPLITTER);
            
            for (Node child : cur.children) {
                queue.offer(child);
            }
        }
    }

    // Decodes your encoded data to tree.
    public Node deserialize(String data) {
        if (data.equals("")) {
            return null;
        }
        
        String[] strings = data.split(",");
        
        // 准备3个Queue，用来处理nodes
        Queue<String> dataQueue = new LinkedList<>();
        for (String s : strings) {
            dataQueue.offer(s);
        }
        int val = Integer.parseInt(dataQueue.poll());
        int size = Integer.parseInt(dataQueue.poll());
        
        Queue<Node> nodeQueue = new LinkedList<>();
        Node root = new Node(val, new ArrayList<Node>());
        nodeQueue.offer(root);
        
        Queue<Integer> sizeQueue = new LinkedList<>();
        sizeQueue.offer(size);
        
        bfsDeser(dataQueue, nodeQueue, sizeQueue);
        
        return root;
    }
    
    private void bfsDeser(Queue<String> dataQueue, Queue<Node> nodeQueue, Queue<Integer> sizeQueue) {
        while (!nodeQueue.isEmpty()) {
            Node cur = nodeQueue.poll();
            int childrenSize = sizeQueue.poll();

            for (int i = 0; i < childrenSize; i++) {
                int val = Integer.parseInt(dataQueue.poll());
                Node child = new Node(val, new ArrayList<Node>());
                cur.children.add(child);
                nodeQueue.offer(child);
                
                int childrenChildrenSize = Integer.parseInt(dataQueue.poll());
                sizeQueue.offer(childrenChildrenSize);
            }
        }
    }
}
```

## Solution 3，速度慢，把树弄成 [1[3[5 6] 2 4]]
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
