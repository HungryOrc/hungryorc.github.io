---
title: "Shortest Path in a Graph that Has Layers, 图分层"
tags: [algorithm, graph]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortestPathInGraph_图分层.html
folder: algorithm
toc: false
---

## Description：Google Onsite 题
给几层点，每两层之间fully connected (比如第一层的任意点和第二层的任意点都**直接**相连， 不相邻的层之间不相连)。
node 有cost，edge 有cost，求第一层的任意点到最后一层的任意点的所有路径中，最小cost是多少

### Example
略

## Solution：BFS
从最上面一层开始，每次只做相邻的两层。最后更新终点层的每一个点的cost，最小的那个就是答案了。具体来说如下，比如说有三层：
* 先看第一层到第二层
  * 看第二层里的某一个点，看第一层的每个点到它的cost，找到最小的那个值，就是我们到达第二层的这个点的最小cost
  * 对于第二层里的每个点，都作上述做法，那么就得到每个点的最小cost
* 再看第二层到第三层。此时第二层每个点的cost就设为从第一层到达这个点的最小cost
  * 根据上面一样的方法，得到到达第三层的每个点的最小cost
  * 这些min costs 里的 min 就是从第一层到第三层的 最小cost

### Complexity
* Time: O(|V| + |E|)，因为每个点和每个边都要考察。然后本图的分层的情况不定，所以|V|和|E|的量级关系不定 <=== ？？？？
* Space: O(n) <=== ？？？？

### Java
逻辑并不算复杂。去掉2个helper classes和最后的main function之后，代码并不长
```java
class Node {
    char label;
    int selfCost;
    // 注意必须区分对待这两种costs！一个node出生时cost可能为5，但考察了从上面下来的到达它的所有paths以后，
    // 这些paths到达它的时候的 path cost 也许分别是 15,25,50，那么这个node的path cost就是15
    int pathCost; 
    List<Edge> edges;
    
    public Node(char l, int sc, int pc) {
        label = l;
        selfCost = sc;
        pathCost = pc;
        edges = new ArrayList<Edge>();
    }
}

class Edge {
    int selfCost;
    Node destiNode;
    
    public Edge(int sc, Node d) {
        selfCost = sc;
        destiNode = d;
    }
}

public class Solution {
    public int shortestPath(Map<Character, Node> layerOneNodes) {
        if (layerOneNodes == null || layerOneNodes.size() == 0) {
            return 0;
        }
        
        // <label, node>，之所以要搞这么一个map，而不是直接把nodes放到一个set里，
        // 是为了避免为 Node class 写 hashCode 和 equals 方法
        Map<Character, Node> curLayerNodes = layerOneNodes;
        Map<Character, Node> nextLayerNodes = new HashMap<>();
        Map<Character, Node> prevLayerNodes = null;
        
        // 最后一层再往下就没有nodes了，所以到了最后一层的再下一层的时候，就会终止这个while loop
        while (curLayerNodes.size() > 0) {
            for (Node curNode : curLayerNodes.values()) {
                // 对于第一层的nodes来说，它们的self cost和path cost必须是一样的，
                // 这是在初始化这些第一层的nodes的时候保证的
                int curNodeCost = curNode.pathCost;
                
                for (Edge edge : curNode.edges) {
                    int curEdgeCost = edge.selfCost;
                    
                    Node destiNode = edge.destiNode;
                    char destiNodeLabel = destiNode.label;
                    
                    Node existingNodeWithThisLabel = nextLayerNodes.get(destiNodeLabel);
                    if (existingNodeWithThisLabel == null) {
                        destiNode.pathCost = curNodeCost + curEdgeCost + destiNode.selfCost; // 别忘了这个
                        nextLayerNodes.put(destiNodeLabel, destiNode);
                    } else {
                        existingNodeWithThisLabel.pathCost = Math.min(
                            existingNodeWithThisLabel.pathCost, 
                            curNodeCost + curEdgeCost + existingNodeWithThisLabel.selfCost);
                    }
                }
            }
            
            prevLayerNodes = curLayerNodes;
            curLayerNodes = nextLayerNodes;
            nextLayerNodes = new HashMap<>();
        }
        
        // 找最后一层nodes里，path cost 最小的一个
        int minPathCost = Integer.MAX_VALUE;
        for (Node node : prevLayerNodes.values()) {
            minPathCost = Math.min(minPathCost, node.pathCost);
        }
        return minPathCost;
    }

    // ------------------------------------------------------
    // main
    // 
    //   a     b
    //    \   /
    //      c
    //    / | \
    //   d  e  f
    //
    public static void main(String[] args) {
        Node a = new Node('a', 10, 10); // 第一层的node，path cost和self cost必须相等
        Node b = new Node('b', 20, 20); // 第一层的node，path cost和self cost必须相等
        Node c = new Node('c', 30, Integer.MAX_VALUE);
        Node d = new Node('d', 40, Integer.MAX_VALUE);
        Node e = new Node('e', 50, Integer.MAX_VALUE);
        Node f = new Node('f', 60, Integer.MAX_VALUE);
        
        Edge ac = new Edge(1, c);
        Edge bc = new Edge(2, c);
        Edge cd = new Edge(3, d);
        Edge ce = new Edge(4, e);
        Edge cf = new Edge(5, f);
        
        List<Edge> edges = new ArrayList<>();
        // for Node a
        edges.add(ac);
        a.edges = new ArrayList<>(edges); // 新new一个list出来
        // for Node b
        edges = new ArrayList<>(); // 清空
        edges.add(bc);
        b.edges = new ArrayList<>(edges);
        // for Node c
        edges = new ArrayList<>();
        edges.add(cd);
        edges.add(ce);
        edges.add(cf);
        c.edges = edges; // 最后一层了，不需要再new一个list了，直接用这个list即可
        
        Map<Character, Node> layerOneNodes = new HashMap<>();
        layerOneNodes.put('a', a);
        layerOneNodes.put('b', b);
        
        Solution solu = new Solution();
        int result = solu.shortestPath(layerOneNodes);
        System.out.println(result); // 84 = 10(a) + 1(ac) + 30(c) + 3(cd) + 40(d)
    }
}
```

## Reference
网上没看见这题

{% include links.html %}
