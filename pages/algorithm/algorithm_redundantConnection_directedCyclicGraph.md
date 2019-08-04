---
title: "Redundant Connection II: In An Directed Cyclic Graph"
tags: [algorithm, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_redundantConnection_directedCyclicGraph.html
folder: algorithm
toc: false
---

## Description
In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a **directed** graph that started as a rooted tree with N nodes (with distinct values 1, 2, ..., N), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] that represents a directed edge connecting nodes u and v, where u is a parent of child v.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

Note:
* The size of the input 2D-array will be between 3 and 1000.
* Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

### Example
* Input: [[1,2], [1,3], [2,3]]
  * Output: [2,3]. The given undirected graph will be like this:
    ```
      1
     /  \
    v    v
    2 -> 3
    ```
* Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
  * Output: [1,4]. The given undirected graph will be like this:
    ```
    5 <- 1 -> 2
         ^    |
         |    v
         4 <- 3
    ```

## Solution: Union Find，速度 前1%
这题我还没懂透，还需要再重复看看

参考：
* 花花解题视频：https://www.youtube.com/watch?v=lnmJT5b4NlM
* Leetcode Discussion: https://leetcode.com/problems/redundant-connection-ii/discuss/112569/Easiest-understanding-Java-Solution-using-Union-Find-O(n).

### Complexity
* Time: O(n) <==== 因该是多少？？？？
* Space: O(n)

### Java
```java
class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {
        int n = edges.length;
        
        int[] roots = new int[n + 1];
        for (int i = 0; i < n + 1; i++) roots[i] = i; // 这样就可以用 1 到 n 来表示roots数组里的nodes

        // 一个有向图，如果不是 valid tree，那么有3种情况：
        // 1. 某个node有两个parents，且图里有环，那么就要确保去掉的edge是环里的一个edge
        // 2. 某个node有两个parents，且图里无环，那么去掉两个重复parent的edges里的任何一个都行
        // 3. 任何node都只有一个parent，且图里有环，这就和无向图里拆掉环那一题是一样的了，即 Redundant Connection I
        int[] candidate1 = null, candidate2 = null;
        
        // 一共要扫描所有edges两次，第一次是找所谓的2个candidate edges，具体见如下comments
        for (int[] edge : edges){
            int from = edge[0], to = edge[1];
            int rootOfFromNode = findRoot(roots, from);
            int rootOfToNode = findRoot(roots, to);
            
            // when an issue happens, we skip the "union" process
                
            // record the last edge which results in "2 parents" issue
            // 因为cur edge是从from node 到 to node 的，所以from就是to的root，而如果to现在已经有了别的root了，
            // （每个node的初始root都是自己，不是自己即意味着有了别的root了），那么就意味着 to node 有了2个root
            if (rootOfToNode != to) candidate1 = edge; 
            // record the last edge which results in "cycle" issue, if any
            else if (rootOfFromNode == rootOfToNode) candidate2 = edge; 
            // no issue happened, we can union now
            else roots[rootOfToNode] = rootOfFromNode;
        }

        // If there is only one issue, return this one.
        if (candidate1 == null) return candidate2; 
        if (candidate2 == null) return candidate1;
        
        // 扫描所有edges第二次，
        /* If both issues present, then the answer should be the first edge which results in "2 parents" issue
        The reason is, when an issue happens, we skip the "union" process.
		Therefore, if both issues happen, it means the incorrent edge which results in "2 parents" was ignored. */
        for (int[] edge : edges) {
            int to = edge[1];
            if (to == candidate1[1]) return edge;
        }

        return new int[]{0, 0};
    }

    private int findRoot(int[] roots, int i){ // 这个平淡无奇
        int node = i;
        while (node != roots[node]){
            int parent = roots[node];
            node = parent;
        }
        return node;
    }
}
```

## Reference
* [Redundant Connection II [LeetCode]](https://leetcode.com/problems/redundant-connection-ii/description/)

{% include links.html %}
