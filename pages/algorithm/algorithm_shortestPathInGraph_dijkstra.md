---
title: "Dijkstra's Shortest Path Algorithm (One Node to All other Nodes in Graph)"
tags: [algorithm, algorithm_overview]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortestPathInGraph_dijkstra.html
folder: algorithm
toc: false
---

## Overview
Dijkastra's Algorithm 是一种 Graph 上的算法，用于计算 从图上的某1点出发，到其他所有点的分别的最短距离
* 所采用的主要数据结构是 Priority Queue (Min Heap)。
* 当起始点到所有点的距离都算完的时候，这个 priority queue 应该是空的
* Cost(source to cur node) = Cost(source to parent node) + Cost(parent to cur node)
* Generation / Expansion rule for the nodes in this algorithm
* **一个node会被放入到PQ里一次或多次，所以也会被弹出PQ一次或多次；因为放入多少次，就必须要弹出多少次，因为PQ最后必须是空的。但当一个node被从PQ里第一次弹出来的时候，它到source node 的最短距离就定了，就是弹出来时候的距离，之后不管它还有多少次被弹出来，距离都不可能比这个第一次被弹出来的时候更短了**。解释：比如和你血缘最近的是亲父母和亲子女，从他们开始向外扩散，得到的那些人和你的血缘关系递减，到了邻居老王的儿子狗剩的时候，血缘关系应该是极为稀薄了，这就叫通过一个很长的路径到达一个node。但是如果另一方面，从你的亲儿子那里出发，其实早就有一条非常短的边直接连到了狗剩（比如狗剩是你儿子的亲哥），那你和狗剩之间的距离其实是比你以为的要短得多。换句话说，从你最紧密的人们出发的BFS，得到的次一级紧密的人(比如狗剩)和你的关系紧密度 T1，是必然要高于或等于 任何和你不那么紧密的人的BFS所得到的狗剩和你的关系紧密度 T2 的
* **从上面的分析可以得到，越早从PQ 里面弹出来的 nodes，它们和 source node 的距离越近，之后从 PQ 里弹出来的nodes，它们和 source node 之间的距离 一定比前面那些的 要远**
* 如果要求一点**到另一个点**的最短距离，而不是一点到多个点的最短距离，那么 算法的结束条件就不是PQ变空了，而是 **要求的终点从PQ里被pop出来**

以下图为例，每个 `Nx` 表示 `Node x`，每个 `--y--` 表示这个是一条边，这条边的长度是 `y`（两个方向走都是距离y）。
```
N6 --4-- N4 --9-- N5 
         |        |   \
         1        2     1
         |        |       \
         N3 --3-- N2 --1-- N1

```
设 N4 为起始点，即要求 N4 到图上所有其他点的最短距离分别是多少。步骤：
* 把 (N4, 0) 放到 priority queue 里去，其中后面那个int 0 表示从source node（即N4）开始到当前node的距离（可能是最终的最短距离，也可能只是计算结果中途的疑似最短距离）。在 PQ 里面，也是靠这个距离值进行排序的。当前 PQ 里距离最小的那个 node，会被排在 PR 的top。
* 在 PQ 里，把top node 即 N4 弹出来。然后更新 N4 的所有邻居的距离，然后把这些邻居都放到 PQ 里去。
  * 所以这些会被放到 PQ 里去：(N6, 4), (N3, 1), (N5, 9)
  * 然后 (N3, 1) 会自然被置于 PQ 的顶端
* 在 PQ 里，再次 pop top，N3 被弹出来了。

### Complexity
* Time: O(n logn)，n 是图里的 nodes 的个数，然后要求nodes的connectivity是constant，那么 Dijkstra 算法的时间复杂度就如左边所写。证明的话在这里可能有：https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm
* Space: O(n)，PQ 的 size
  
## Implementation
* 用 Integer 代表 nodes，因为任何复杂的 object 都可以用 indexes 来表示
* Graph 用一个map来表示，map的key是node id，value是另一个map <neighbor node id, distance from cur node to this neighbor node>
* Priority Queue 里放的东西是我自定义的一个helper class "NodeDist"，这个class里有2个成员：node id 和 distance from source node to cur node。PQ 的排序规则是按照 distance 的升序排列

思路并不复杂，代码也不长，去掉前面的helper function 和后面的 main function，主干的 Dijkstra 逻辑 也就三四十行
```java
// helper class
// 别忘了写Comparable后面的<NodeDist>！否则后面的 compareTo 函数的参数就
// 必须是 Object type 了，而不能是 NodeDist type 了
class NodeDist implements Comparable<NodeDist> {
    int nodeId, distFromSource;
    
    public NodeDist(int id, int dist) {
        nodeId = id;
        distFromSource = dist;
    }
    
    @Override
    public int compareTo(NodeDist nd) {
        if (this.distFromSource == nd.distFromSource) {
            return 0;
        }
        return this.distFromSource > nd.distFromSource ? 1 : -1;
    }
}

public class Solution {
    // 要求返回的是一个map，key是node id，value是source node 到这个node的 min distance
    public Map<Integer, Integer> dijkstra(
            Map<Integer, Map<Integer, Integer>> graph, int sourceNode) {
        // ignore data validations
        
        Map<Integer, Integer> result = new HashMap<>();
        
        // Java里的PQ默认是 min heap，要 max heap 的话，要写：PriorityQueue<Integer> pq = 
        // new PriorityQueue<>(10, Collections.reverseOrder());
        PriorityQueue<NodeDist> minHeap = new PriorityQueue<>();
        
        NodeDist sourceND = new NodeDist(sourceNode, 0);
        minHeap.add(sourceND);
        
        while (!minHeap.isEmpty()) {
            NodeDist cur = minHeap.poll();
            int curNode = cur.nodeId;
            int curDist = cur.distFromSource;
            
            // 注意！如果当前这个node之前已经被PQ pop过了，那么现在再被pop出来，
            // 一定是距离source node 比之前的那次pop的更远，所以本次pop没有意义，直接抛弃
            if (result.containsKey(curNode)) {
                continue;
            }
            result.put(curNode, curDist);
            
            Map<Integer, Integer> neighbors = graph.get(curNode);
            for (Map.Entry<Integer, Integer> entry : neighbors.entrySet()) {
                int neiId = entry.getKey();
                int neiDist = entry.getValue();

                if (result.containsKey(neiId)) {
                    continue;
                }
                
                // 别忘了加上当前node的dist！当前node算是 parent node
                NodeDist neiND = new NodeDist(neiId, neiDist + curDist);
                minHeap.add(neiND);
            }
        }
        return result;
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {
        Map<Integer, Map<Integer, Integer>> graph = new HashMap<>();
        
        Map<Integer, Integer> neighbors4 = new HashMap<>();
        neighbors4.put(6, 4);
        neighbors4.put(3, 1);
        neighbors4.put(5, 9);
        graph.put(4, neighbors4);
        
        Map<Integer, Integer> neighbors3 = new HashMap<>();
        neighbors3.put(4, 1);
        neighbors3.put(2, 3);
        graph.put(3, neighbors3);
        
        Map<Integer, Integer> neighbors5 = new HashMap<>();
        neighbors5.put(4, 9);
        neighbors5.put(1, 1);
        neighbors5.put(2, 2);
        graph.put(5, neighbors5);
        
        Map<Integer, Integer> neighbors6 = new HashMap<>();
        neighbors6.put(4, 4);
        graph.put(6, neighbors6);        
        
        Map<Integer, Integer> neighbors2 = new HashMap<>();
        neighbors2.put(1, 1);
        neighbors2.put(3, 3);
        neighbors2.put(5, 2);
        graph.put(2, neighbors2); 

        Map<Integer, Integer> neighbors1 = new HashMap<>();
        neighbors1.put(2, 1);
        neighbors1.put(5, 1);
        graph.put(1, neighbors1);
        
        Solution solu = new Solution();
        Map<Integer, Integer> result = solu.dijkstra(graph, 4);
        
        System.out.println(result);
        // 正确答案：{1=5, 2=4, 3=1, 4=0, 5=6, 6=4}，前面是node，后面是distance
    }
} 
```

## Reference
网上没看到直接让写 Dijkstra's Algorithm 的题目
* [Dijkstra's Algorithm [WikiPedia]](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

{% include links.html %}
