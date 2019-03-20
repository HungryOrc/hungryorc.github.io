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
    List<DirectedGraphNode> neighbors;
    
    public DirectedGraphNode(int x) { 
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

## Solution：速度 前30%
1. 计算所有点的 in-degree
2. 把所有 in-degree == 0 的点都放到queue里去，也放到result里去
3. 从queue里poll一个node出来，如果这个node的inDegree是0，就把它的所有neighbors 的 in-degree 都减1，包括in-degree已经为零甚至为负数的点！因为成为0只能最多有一次！
4. 如果任何neighbor 的 inDegree在上述的-1以后变成了0，就把这个neighbor 放到queue里去
5. 重复上述过程，直至queue空

注意：
* 天生就入度为0的nodes，它们一定是第一批被放入result的，也是第一批被放入queue的。它们所指向的入度为1的nodes，
就是第2批应该放入 result 以及 queue 的nodes（因为这些nodes也将是第1批被转化为入度零的nodes）。
* 对于天生都是入度为1的nodes，它们的“前置node”的天生入度有可能是0，也有可能大于0，而且它们有可能有不止一个前置nodes。这些因素就决定了，它们虽然都是天生入度为1，但它们进入result（以及queue）的排位可能会相差很远

**特别注意！如果graph里有cycle，则要么所有的nodes一开始的入度都不为0，意味着所有nodes都在一个cycle上；要么部分nodes一开始的入度为0，但处理到一定时候以后，会发现剩下的nodes都无法装到while loop里去了，而这些无法进去的nodes此时的入度都 > 0，意味着剩下的这些nodes组成了一个cycle。所以，最后，在result里的nodes个数如果小于graph里的nodes个数，那么就意味着graph里一定有环！**

### Complexity
* Time: O(n) <==== 对么 ？？？？
* Space: O(n)，queue size

### Java
```java
public class Solution {
    public List<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
        List<DirectedGraphNode> result = new ArrayList<>();
        if (graph == null || graph.size() == 0) {
            return result;
        }
        
        // build a map to store the in-degree count of all the nodes
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
            // 先把天生inDegree就是零的点(们)放到queue里，
            // 同时也把它们放到result里，它们就是排在最前面的那些nodes
            if (inDegreeMap.get(node) == 0) {
                queue.offer(node);
                result.add(node);
            }
        }
        
        while (!queue.isEmpty()) {
            DirectedGraphNode node = queue.poll();
           
            // 对于cur node 的每个 neighbor，上来二话不说，先把它们的 inDegree 都减1
            for (DirectedGraphNode nei : node.neighbors) {
                int neiInDegree = inDegreeMap.get(nei);
                neiInDegree--;
                inDegreeMap.put(nei, neiInDegree);

                if (neiInDegree == 0) {
                    queue.offer(nei);
                    result.add(nei);
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
