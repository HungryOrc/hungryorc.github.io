---
title: "Clone Graph"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_clone_graph.html
folder: algorithm
toc: false
---

## Description
Given the head of a graph, return a deep copy (clone) of the graph. Each node in the graph contains a label (int) and a list (List[UndirectedGraphNode]) of its neighbors. There is an edge between the given node and each of the nodes in its neighbors.

Visually, the graph might look like the following:
```
   1
  / \
 /   \
0 --- 2
     / \
     \_/
```

Definition for undirected graph.
```java
class UndirectedGraphNode {
    int label;
    ArrayList<UndirectedGraphNode> neighbors;
    
    public UndirectedGraphNode(int x) { 
        label = x; 
        neighbors = new ArrayList<UndirectedGraphNode>(); 
    }
};
```

注意：
* 这题默认所有的nodes都是联通在一起的
* graph里可能会有cycle，但这题的解法可以无视有没有cycle的问题

### Example
* Input: 
  * Output: 

## Solution
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
