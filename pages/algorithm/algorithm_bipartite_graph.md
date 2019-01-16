---
title: "Is Graph Bipartite"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_bipartite_graph.html
folder: algorithm
toc: false
---

## Description
Given an undirected graph, return true if and only if it is bipartite.

Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: graph[i] is a list of indexes j for which the edge between nodes i and j exists. Each node is an integer between 0 and graph.length - 1. There are no self edges or parallel edges: graph[i] does not contain i, and it doesn't contain any element twice.
* graph will have length in range [1, 100].
* graph[i] will contain integers in range [0, graph.length - 1].
* graph[i] will not contain i or duplicate values.
* The graph is undirected: if any element j is in graph[i], then i will be in graph[j].

### Example
* Input: [[1,3], [0,2], [1,3], [0,2]]
  * Output: true. The graph looks like the following, we can divide the vertices into two groups: {0, 2} and {1, 3}.
    ```
    0----1
    |    |
    |    |
    3----2
    ```
* Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
  * Output: false. The graph looks like the following, we cannot find a way to divide the set of nodes into two independent subsets.
    ```
    0----1
    | \  |
    |  \ |
    3----2
    ```

## Solution
很自然的想法

### Complexity
* Time: O(n)，n 是 edges的个数
* Space: O(n)

### Java
```java
public class Solution {
    public boolean isBipartite(int[][] graph) {
        if (graph == null || graph.length == 0) {
            return true; // 这题要求如此
        }
        
        // <integer represents the node, char represents the group A or B>
        Map<Integer, Character> groups = new HashMap<>();
        Queue<Integer> nodes = new LinkedList<>();
        
        for (int i = 0; i < graph.length; i++) {
            char group = groups.getOrDefault(i, 'A');
            groups.put(i, group);
            nodes.offer(i);
        
            while (!nodes.isEmpty()) {
                int curNode = nodes.poll();
                char curGroup = groups.get(curNode);
                
                int[] neighbors = graph[curNode];
                for (int nei : neighbors) {
                    char neiGroup = groups.getOrDefault(nei, ' ');
                    if (neiGroup == curGroup) {
                        return false;
                    } else if (neiGroup == ' ') {
                        char setGroup = getOppositeGroup(curGroup);
                        groups.put(nei, setGroup);
                    } // else neiGroup != curGroup, then we do nothing
                }
            }
        }
        return true;
    }
    
    private char getOppositeGroup(char group) {
        if (group == 'A') {
            return 'B';
        }
        return 'A';
    }
}
```

## Reference
* [Is Graph Bipartite [LeetCode]](https://www.lintcode.com/problem/is-graph-bipartite/description)

{% include links.html %}
