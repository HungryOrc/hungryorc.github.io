---
title: "Network Delay Time"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_network_delay_time.html
folder: algorithm
toc: false
---

## Description
There are N network nodes, labelled 1 to N.

Given times, a list of travel times as directed edges times[i] = (u, v, w), where u is the source node, v is the target node, and w is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node K. How long will it take for all nodes to receive the signal? If it is impossible, return -1.

### Example
* Input: times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2。配图见 https://leetcode.com/problems/network-delay-time/description/
  * Output: 2

## Solution 这题其实就是一种 Dijkstra

### Complexity
* Time: O(|E| + |V|)  <=== 对么？？
* Space: O(|E| + |V|) <=== 对么？？

### Java
```java
class Node implements Comparable<Node> {
    int index;
    int distanceFromSource;
    
    public Node(int i, int d) {
        this.index = i;
        this.distanceFromSource = d;
    }
    
    @Override
    public int compareTo(Node n2) {
        return Integer.compare(this.distanceFromSource, n2.distanceFromSource);
    }
}

public class Solution {
    public int networkDelayTime(int[][] times, int n, int source) {
        // ignore sanity checks
        
        int overallTime = 0;
        
        // <index of node, <index of neighbor, distance from this node to neighbor>>
        Map<Integer, Map<Integer, Integer>> graph = new HashMap<>();
        // <index of node, distance from source node to this node>
        Map<Integer, Integer> distsFromSource = new HashMap<>();
        
        // transfer the data from the matrix to the Map of the Nodes
        for (int i = 0; i < times.length; i++) {
            int[] edge = times[i];
            int fromIndex = edge[0];
            int toIndex = edge[1];
            int distance = edge[2];
            
            Map<Integer, Integer> neighbors = graph.get(fromIndex);
            if (neighbors == null) {
                neighbors = new HashMap<>();
                neighbors.put(toIndex, distance);
                graph.put(fromIndex, neighbors);
            }
            else { 
                neighbors.put(toIndex, distance);
            }
        }

        // Java里的PQ默认是 min heap，要 max heap 的话，要写：PriorityQueue<Integer> pq = 
        // new PriorityQueue<>(10, Collections.reverseOrder());
        PriorityQueue<Node> minHeap = new PriorityQueue<>();
        Node sourceNode = new Node(source, 0);
        minHeap.offer(sourceNode);
        
        while (!minHeap.isEmpty()) {
            Node curNode = minHeap.poll();
            int curIndex = curNode.index;
            int curDistFromSource = curNode.distanceFromSource;
     
            // 这题的关键所在！！我曾经错在这里很久！
            // 如果当前这个node之前已经被PQ pop过了，那么现在再被pop出来，
            // 一定是距离source node 比之前的那次pop的更远，所以本次pop没有意义，直接抛弃
            if (distsFromSource.containsKey(curIndex)) {
                continue;
            }            
            distsFromSource.put(curIndex, curDistFromSource);
            overallTime = Math.max(overallTime, curDistFromSource);
            
            Map<Integer, Integer> neighbors = graph.get(curIndex);
            if (neighbors == null) continue;
            for (Map.Entry<Integer, Integer> entry : neighbors.entrySet()) {
                int nextIndex = entry.getKey();
                int distance = entry.getValue();
                
                if (distsFromSource.containsKey(nextIndex)) {
                    continue;
                }
                
                Node nextNode = new Node(nextIndex, curDistFromSource + distance);
                minHeap.offer(nextNode);
            }
        }
        
        if (distsFromSource.size() < n) {
            return -1;
        }
        return overallTime;
    }
}
```

## Reference
* [Network Delay Time [LeetCode]](https://leetcode.com/problems/network-delay-time/description/)

{% include links.html %}
