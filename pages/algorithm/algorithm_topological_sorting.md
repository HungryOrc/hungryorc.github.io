---
title: "Topological Sorting"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_topological_sorting.html
folder: algorithm
toc: false
---

## Description
Given an directed graph, a topological order of the graph nodes is defined as follow:
For each directed edge A -> B in graph, A must before B in the order list.
The first node in the order can be any node in the graph with no nodes direct to it.
Find any topological order for the given graph.
```java
// Definition for Directed graph:
class DirectedGraphNode {
    int label;
    ArrayList<DirectedGraphNode> neighbors;
    DirectedGraphNode(int x) { 
        label = x; 
        neighbors = new ArrayList<DirectedGraphNode>(); 
    }
}
```

Notice: 
* You can assume that there is at least one topological order in the graph.
* 每一条有向边，即每一个node的neighbors关系，当前node就是有向边的起点，每一个neighbor都各是一个终点。
* 如果点A有一个neighbor为点B，则点B并没有neighbor是点A，除非它们构成循环；这个例子里，B的 in-degree 就多了1，而A的 in-degree 没受影响

### Example
For graph as follow: <此处有配图>
本题链接：https://www.lintcode.com/problem/topological-sorting/description

## Solution：我现在这个写法思路是对的，但速度较慢，回头看下有没有更快的写法
1 计算所有点的 in-degree
2 把所有 in-degree == 0 的点都放到queue里去
3 从queue里poll一个node出来，如果这个node的inDegree是0，就把它的所有neighbors 的 in-degree 都减1，包括in-degree已经为零甚至为负数的点！因为成为0只能最多有一次！
4 如果任何neighbor 的 inDegree在上述的-1以后变成了0，就把这个neighbor 放到queue里去
5 重复上述过程，直至queue空


### Complexity
* Time: O(n) <==== 对么 ？？？？
* Space: O(n)，queue size

### Java
```java
public class Solution {
    public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
        ArrayList<DirectedGraphNode> result = new ArrayList<>();
        if (graph == null || graph.size() == 0) {
            return result;
        }
        
        // build a map of the in-degree count of all the nodes
        // <node, inDegree>
        Map<DirectedGraphNode, Integer> inDegreeMap = new HashMap<>();
        for (DirectedGraphNode node : graph) {
            // 注意！下面这步不能少，因为有的node可能不是任何node的neighbor
            if (!inDegreeMap.containsKey(node)) {
                inDegreeMap.put(node, 0);
            }
            
            for (DirectedGraphNode nei : node.neighbors) {
                inDegreeMap.put(nei, inDegreeMap.getOrDefault(nei, 0) + 1);
            }
        }
        
        Queue<DirectedGraphNode> queue = new LinkedList<>();
        for (DirectedGraphNode node : inDegreeMap.keySet()) {
            if (inDegreeMap.get(node) == 0) {
                queue.offer(node);
            }
        }
        
        while (!queue.isEmpty()) {
            DirectedGraphNode node = queue.poll();
            int inDegree = inDegreeMap.get(node);
            
            if (inDegree == 0) {
                result.add(node);

                for (DirectedGraphNode nei : node.neighbors) {
                    int neiInDegree = inDegreeMap.get(nei);
                    neiInDegree--;
                    inDegreeMap.put(nei, neiInDegree);
                    
                    if (neiInDegree == 0) {
                        queue.offer(nei);
                    }
                }
            }
        }
        return result;
    }
}
```

## Reference
* [Topological Sorting [LintCode]](https://www.lintcode.com/problem/topological-sorting/description)

{% include links.html %}
