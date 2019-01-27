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
Given the head of a graph, return a deep copy (clone) of the graph. Each node in the graph contains a `label` (`int`) and a list (`List[UndirectedGraphNode]`) of its `neighbors`. There is an edge between the given node and each of the nodes in its neighbors.

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
    List<UndirectedGraphNode> neighbors;
    
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
略

## Solution 1：最符合直觉的方法，一步一步来，容易理解，但速度较慢


### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public class Solution {
    // 题目只给一个node的话，意味着默认所有nodes都是连接可达的
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) {
            return null;
        }
        
        List<UndirectedGraphNode> oldNodes = getOldNodes(node);
        
        // <old node, new node which is the deep copy of the old node>
        Map<UndirectedGraphNode, UndirectedGraphNode> oldToNew = new HashMap<>();
        
        for (UndirectedGraphNode oldNode : oldNodes) {
            oldToNew.put(oldNode, new UndirectedGraphNode(oldNode.label));
        }
        
        for (UndirectedGraphNode oldNode : oldNodes) {
            UndirectedGraphNode newNode = oldToNew.get(oldNode);
            
            for (UndirectedGraphNode nei : oldNode.neighbors) {
                newNode.neighbors.add(oldToNew.get(nei));
            }
        }
        
        return oldToNew.get(node);
    }
    
    private List<UndirectedGraphNode> getOldNodes(UndirectedGraphNode node) {
        Set<UndirectedGraphNode> set = new HashSet<>();
        Queue<UndirectedGraphNode> queue = new LinkedList<>();
        set.add(node);
        queue.offer(node);
        
        while (!queue.isEmpty()) {
            UndirectedGraphNode cur = queue.poll();
            
            for (UndirectedGraphNode nei : cur.neighbors) {
                if (!set.contains(nei)) {
                    set.add(nei);
                    queue.offer(nei);
                }
            }
        }
        return new ArrayList<UndirectedGraphNode>(set);
    }
}
```

## Reference
* [Clone Graph [LeetCode]](https://leetcode.com/problems/clone-graph/description/)

{% include links.html %}
