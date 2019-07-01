---
title: "Cheapest Flights Within K Stops"
tags: [algorithm, graph]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cheapest_flights_within_k_stops.html
folder: algorithm
toc: false
---

## Description
There are n cities connected by m flights. Each fight starts from city u and arrives at v with a price w.

Now given all the cities and flights, together with starting city src and the destination dst, your task is to find the cheapest price from src to dst with up to k stops. If there is no such route, output -1.

Note:
* The number of nodes n will be in range [1, 100], with nodes labeled from 0 to n - 1.
* The size of flights will be in range [0, n * (n - 1) / 2].
* The format of each flight will be (src, dst, price).
* The price of each flight will be in the range [1, 10000].
* k is in the range of [0, n - 1].
* There will not be any duplicated flights or self cycles.

### Example
见原题：https://leetcode.com/problems/cheapest-flights-within-k-stops/description/

## Solution
这题就是一个稍微变形的Dijkstra。多的东西是 步数限制，具体的应对方法见下面code里的注释

### Complexity
* Time: O(|E| log|E|) <==== 对么？？？
* Space: O(|E|) <==== 对么？？？

### Java
代码看起来不短，其实思路不复杂。
```java
class Node implements Comparable<Node> {
    int index;
    int distFromSrc;
    int steps;
    
    public Node(int i, int d, int s) {
        this.index = i;
        this.distFromSrc = d;
        this.steps = s;
    }
    
    @Override
    public int compareTo(Node n2) {
        return Integer.compare(this.distFromSrc, n2.distFromSrc);
    }
}

public class Solution {
    // 这里的k是指中间换乘的次数！所以k=2的话意味着一共允许最多走3步！
    // 所以k可以等于 0
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        if (n <= 0 || flights == null || flights.length == 0 || flights[0].length == 0 ||
                src < 0 || dst < 0 || k < 0) {
            return -1;
        }
        
        // <city index, <neighbor city index, distance>>
        Map<Integer, Map<Integer, Integer>> graph = new HashMap<>();
        
        // 把所有的edge的信息都录入到graph里去
        for (int i = 0; i < flights.length; i++) {
            int[] flight = flights[i];
            int fromIdx = flight[0];
            int toIdx = flight[1];
            int distance = flight[2];
            
            Map<Integer, Integer> neighbors = graph.get(fromIdx);
            if (neighbors == null) {
                neighbors = new HashMap<>();
            }
            neighbors.put(toIdx, distance);
            graph.put(fromIdx, neighbors);
        }
        
        PriorityQueue<Node> minHeap = new PriorityQueue<>();
        Node srcNode = new Node(src, 0, 0);
        minHeap.offer(srcNode);

        while (!minHeap.isEmpty()) {
            Node curNode = minHeap.poll();
            int curIdx = curNode.index;
            int curDistFromSrc = curNode.distFromSrc;
            int curSteps = curNode.steps;
            
            if (curSteps > k + 1) { // k是最多的中转次数，所以最多的步数是 k+1
                continue;
            }

            if (curIdx == dst) {
                return curDistFromSrc;
            }
            
            // 这一题最重要的关键点是下面这段话！
            // 如果一个node已经被访问过了，它之后再出现的话，只要没超过步数限制，还是要把它再次放到PQ里的！
            // 比如，限制是5步，我们最终要去node C，
            // 对于某个node A，我们5步到了它，累计距离10，但它不能再往下走了，但此时我们不要屏蔽以后的A！
            // 可能以后我们能通过别的路径再次到达A，累计距离一定会更大，但累计步数可能会更小！
            // 这就为从A到C埋伏了可能性！
            // 比如之后有一种走法，到A用了3步，累计距离27，远高于前面的情况，但如果
            // 从A开始2步之内能走到C，那么就胜利了！那么之前夭折A的话就错了
            
            Map<Integer, Integer> neighbors = graph.get(curIdx);
            if (neighbors == null) { // 不存在从这个点出发的edge
                continue;
            }
            
            for (Map.Entry<Integer, Integer> entry : neighbors.entrySet()) {
                int neiIdx = entry.getKey();
                int dist = entry.getValue();
                Node neiNode = new Node(neiIdx, curDistFromSrc + dist, curSteps + 1);
                minHeap.offer(neiNode);
            }
        }
        return -1;
    }
}
```

## Reference
* [algorithm_cheapest_flights_within_k_stops [LeetCode]](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)

{% include links.html %}
