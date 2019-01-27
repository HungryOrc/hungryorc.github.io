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
1. 题目只给了一个node，通过它，把所有的 old nodes 都得到
2. 搞一个map，key是所有的old nodes，value是所有的new nodes，old和new nodes一一对应，后者的label和前者的一一相等。这时候都还没有添加neighbors的数据
3. 对每个new node添加neighbors的数据，当然是根据它所对应的那个old node 的neighbors来添加

### Complexity
* Time: O(|V| + |E|) <=== 对么 ？？？？
* Space: O(|V|) <=== 对么 ？？？？

### Java
```java
public class Solution {
    // 题目只给一个node的话，意味着默认所有nodes都是连接可达的
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) {
            return null;
        }
        
        // from the given node, get all the old nodes in the initial graph
        List<UndirectedGraphNode> oldNodes = getOldNodes(node);
        
        // map each old node with a new node, whose label is the same to the old one
        // 在这些一对一的mapping里，old nodes是key，new nodes是value
        Map<UndirectedGraphNode, UndirectedGraphNode> oldToNew = new HashMap<>();
        
        for (UndirectedGraphNode oldNode : oldNodes) {
            // 到这里为止，new nodes都还没有添加neighbors的信息
            oldToNew.put(oldNode, new UndirectedGraphNode(oldNode.label));
        }
        
        // 把neighbors关系也复制到new nodes里面去
        // 注意，new nodes 的neighbors关系 是存在于 new nodes 和 new nodes 之间的
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
        // 注意这句语法：ArrayList 可以直接由 HashSet 来构造
        return new ArrayList<UndirectedGraphNode>(set);
    }
}
```

## Reference
* [Clone Graph [LeetCode]](https://leetcode.com/problems/clone-graph/description/)

{% include links.html %}
