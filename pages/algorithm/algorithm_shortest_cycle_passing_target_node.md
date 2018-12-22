---
title: "Find the Shortest Cycle that Passes a Specific Node in the Graph"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortest_cycle_passing_target_node.html
folder: algorithm
toc: false
---

## Description
给一个directed graph，再给定图里的某一个node作为target，find the shortest cycle that passes this target node，
return the nodes in this cycle as a list。
如果没有任何cycle通过这个node，则返回空的list。

### Example
略

## Solution: DFS <---- 我的方法对么？？？？

### Complexity
* Time: O(|V| + |E|) <--- 对么 ？？？？
* Space: O(|V|) <--- 对么 ？？？？

### Java
```java
class Node {
    char val; // 这个其实用不上
    List<Node> neighbors;
    
    public Node() {
        this.val = '';
        this.neighbors = new ArrayList<>();
    }
}

public class Solution {
    public List<Node> shortestCycle(Node target) {
    
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
