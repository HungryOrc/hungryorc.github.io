---
title: "Find Eventual Safe States"
tags: [algorithm, topological_sort]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_eventual_safe_states.html
folder: algorithm
toc: false
---

## Description
In a directed graph, we start at some node and every turn, walk along a directed edge of the graph.  If we reach a node that is terminal (that is, it has no outgoing directed edges), we stop.

Now, say our starting node is eventually safe if and only if we must eventually walk to a terminal node.  More specifically, there exists a natural number K so that for any choice of where to walk, we must have stopped at a terminal node in less than K steps.

Which nodes are eventually safe?  Return them as an array in sorted order.

The directed graph has N nodes with labels 0, 1, ..., N-1, where N is the length of graph.  The graph is given in the following form: graph[i] is a list of labels j such that (i, j) is a directed edge of the graph.

Note:
* `graph` will have length at most 10000.
* The number of edges in the graph will not exceed 32000.
* Each graph[i] will be a sorted list of different integers, chosen within the range [0, graph.length - 1].

### Example
* Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
  * Output: [2,4,5,6]. [Here](https://leetcode.com/problems/find-eventual-safe-states/description/) is a diagram of the above graph.

## Solution：很明显就用 topological order 来做。

### Complexity
* Time: O(|V| + |E| + |V|*log(|V|))，其中 |V|*log(|V|) 是按题目要求的最后的排序
* Space: O(|V| + |E|)

### Java
思路简明。
```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        // 这题有个特殊情况：graph[0].length == 0 不能作为 corner case
        if (graph == null || graph.length == 0) {
            return new ArrayList<>();
        }
        
        int n = graph.length;
        Map<Integer, Set<Integer>> inboundEdges = new HashMap<>();
        for (int i = 0; i < n; i++) inboundEdges.put(i, new HashSet<Integer>());
        Map<Integer, Set<Integer>> outboundEdges = new HashMap<>();
        for (int i = 0; i < n; i++) outboundEdges.put(i, new HashSet<Integer>());
        
        // 把2个map都填好
        for (int i = 0; i < n; i++) {
            int fromNode = i;
            int[] toNodes = graph[i];
            
            for (int node : toNodes) {
                Set<Integer> inEs = inboundEdges.get(node);
                inEs.add(i);
                inboundEdges.put(node, inEs);
                
                Set<Integer> outEs = outboundEdges.get(i);
                outEs.add(node);
                outboundEdges.put(i, outEs);
            }
        }
        
        Queue<Integer> queue = new LinkedList<>();
        List<Integer> result = new ArrayList<>();
        
        // 把safe的destinations都放到queue里去
        for (Map.Entry<Integer, Set<Integer>> entry : outboundEdges.entrySet()) {
            if (entry.getValue().isEmpty()) {
                queue.offer(entry.getKey());
                result.add(entry.getKey());
            }
        }
        
        while (!queue.isEmpty()) {
            int curNode = queue.poll();
            
            for (int prevNode : inboundEdges.get(curNode)) {
                Set<Integer> outboundEdgesFromPrevNode = outboundEdges.get(prevNode);
                outboundEdgesFromPrevNode.remove(curNode);
                if (outboundEdgesFromPrevNode.isEmpty()) {
                    result.add(prevNode);
                    queue.offer(prevNode);
                }
            }
        }
        Collections.sort(result);
        return result;
    }
}
```

## Reference
* [Find Eventual Safe States [LeetCode]](https://leetcode.com/problems/find-eventual-safe-states/description/)

{% include links.html %}
