---
title: "Connecting Cities With Minimum Cost"
tags: [algorithm, minimum_spanning_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_connecting_cities_with_min_cost.html
folder: algorithm
toc: false
---

## Description
There are N cities numbered from 1 to N.

You are given connections, where each connections[i] = [city1, city2, cost] represents the cost to connect city1 and city2 together.  (A connection is bidirectional: connecting city1 and city2 is the same as connecting city2 and city1.)

Return the minimum cost so that for every pair of cities, there exists a path of connections (possibly of length 1) that connects those two cities together.  The cost is the sum of the connection costs used. If the task is impossible, return -1.

例子和各种图 [在此](https://leetcode.com/problems/connecting-cities-with-minimum-cost/description/)

Note:
* 1 <= N <= 10000
* 1 <= connections.length <= 10000
* 1 <= connections[i][0], connections[i][1] <= N
* 0 <= connections[i][2] <= 10^5
* connections[i][0] != connections[i][1]

### Example
见上文中的链接

## Solution：我自己的实现，用 Minimum Spanning Tree，Kruskal方法（Union Find实现），速度 前5%

### Complexity
* Time: O(n) <==== ？？？？是n的平方么
* Space: O(n) <==== ？？？？

### Java
```java
class Edge implements Comparable<Edge> {
    int start, end;
    int length;
    
    public Edge(int start, int end, int length) {
        this.start = start;
        this.end = end;
        this.length = length;
    }
    
    @Override
    public int compareTo(Edge e2) {
        return Integer.compare(this.length, e2.length);
    }
}

class ArrayUnionFind {
    int n;
    int[] groupIDs;
    
    public ArrayUnionFind(int n) {
        this.n = n;
        this.groupIDs = new int[n];
        for (int i = 0; i < n; i++) groupIDs[i] = i;
    }
    
    // path compression
    public int getGroupID(int i) {
        int cur = i, groupID = i;
        while (groupIDs[cur] != cur) {
            cur = groupIDs[cur];
        }
        groupID = cur;
        
        cur = i;
        while (groupIDs[cur] != cur) {
            int parentID = groupIDs[cur];
            groupIDs[parentID] = groupID;
            cur = parentID;
        }
        
        return groupID;
    }
    
    public boolean find(int i, int j) {
        return getGroupID(i) == getGroupID(j);
    }
    
    public void union(int i, int j) {
        int groupID1 = getGroupID(i);
        int groupID2 = getGroupID(j);
        
        groupIDs[groupID1] = groupID2;
    }
}

public class Solution {
    public int minimumCost(int n, int[][] connections) {
        if (connections == null || connections.length == 0 || connections[0].length == 0 || n <= 0) {
            return -1;
        }
        
        // 如果edges的个数 不够连接所有的 n 个 nodes，那么最后必然是失败的。
        // 要连接n个nodes，至少我们需要 n-1 条边
        if (connections.length < n - 1) return -1;
        
        int numOfConnectedNodes = 1;
        int totalCost = 0;
        
        List<Edge> allEdges = new ArrayList<>();
        for (int[] edge : connections) {
            int start = edge[0];
            int end = edge[1];
            int length = edge[2];
            
            Edge e = new Edge(start, end, length);
            allEdges.add(e);
        }
        Collections.sort(allEdges);
        
        ArrayUnionFind uf = new ArrayUnionFind(n + 1); // 因为这些cities都是从1开始编号的，不是从0开始
        
        for (Edge e : allEdges) {
            int start = e.start;
            int end = e.end;
            
            if (!uf.find(start, end)) {
                uf.union(start, end);
                numOfConnectedNodes++;
                totalCost += e.length;
            }
        }
        
        if (numOfConnectedNodes < n) return -1; // 最后 有的node还是没有连进来
        return totalCost;
    }
}
```

## Reference
* [Connecting Cities With Minimum Cost [LeetCode]](https://leetcode.com/problems/connecting-cities-with-minimum-cost/description/)

{% include links.html %}
