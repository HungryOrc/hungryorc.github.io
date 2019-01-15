---
title: "Number of Connected Components in an Undirected Graph"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_numOf_connectedComponents_in_undirectedGraph.html
folder: algorithm
toc: false
---

## Description
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

### Example
* Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]
  ```
  0          3
  |          |
  1 --- 2    4 
  ```
  * Output: 2
* Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]
  ```
  0          3
  |          |
  1 --- 2--- 4 
  ```
  * Output: 1

## Solution：速度较慢，回头看下有没有更好的方法
朴素思路

### Complexity
* Time: O(n)，n 是 edges的个数
* Space: O(n)

### Java
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        if (n <= 0) {
            return 0;
        } 
        
        // 特别注意下面这几种特殊情况！这些意味着没有边，但是点(n)有！
        if (edges == null|| edges.length == 0 || edges[0].length == 0) {
            return n;
        }
        
        int count = 0;
        Set<Integer> visited = new HashSet<>();
        Map<Integer, Set<Integer>> edgeMap = new HashMap<>();
        
        // build the map of the edges
        for (int i = 0; i < edges.length; i++) {
            int first = edges[i][0], second = edges[i][1];
            
            buildMuturalEdge(first, second, edgeMap);
            buildMuturalEdge(second, first, edgeMap);
        }
        
        for (int first : edgeMap.keySet()) {
            if (!visited.contains(first)) {
                visited.add(first);
                getWholeComponent(edgeMap, first, visited);
                count++;
            }
        }
        
        // 注意！connected components 以外可能有落单的
        if (visited.size() < n) {
            count += n - visited.size();
        }
        
        return count;
    }
    
    private void buildMuturalEdge(int first, int second, 
                                  Map<Integer, Set<Integer>> edgeMap) {
        Set<Integer> neighbors = 
            edgeMap.getOrDefault(first, new HashSet<Integer>());
        neighbors.add(second);
        edgeMap.put(first, neighbors);
    }
    
    private void getWholeComponent(Map<Integer, Set<Integer>> edgeMap, 
                                   int curNode, Set<Integer> visited) {
        Queue<Integer> nodeQueue = new LinkedList<>();
        nodeQueue.offer(curNode);
        
        while (!nodeQueue.isEmpty()) {
            int cur = nodeQueue.poll();
            Set<Integer> neighbors = edgeMap.get(cur);

            if (neighbors == null) {
                continue;
            }
            
            for (int neighbor : neighbors) {
                if (!visited.contains(neighbor)) {
                    nodeQueue.offer(neighbor);
                    visited.add(neighbor);
                }
            }
        }
    } 
}

```

## Reference
* [Number of Connected Components in an Undirected Graph [LeetCode]](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/description/)

{% include links.html %}
