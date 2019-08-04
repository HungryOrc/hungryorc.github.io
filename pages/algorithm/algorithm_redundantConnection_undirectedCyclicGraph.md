---
title: "Redundant Connection I: In An Undirected Cyclic Graph"
tags: [algorithm, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_redundantConnection_undirectedCyclicGraph.html
folder: algorithm
toc: false
---

## Description
In this problem, a tree is an undirected graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] with u < v, that represents an undirected edge connecting nodes u and v.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge [u, v] should be in the same format, with u < v.

这个array of edges 可以认为是一条边一条边地逐个加到一个图里去，那么如果我们加到某一条边的时候，发现构成环了，那么这条边就是构成这个环的“最后一块拼图”，就得返回这条边

Note:
* The size of the input 2D-array will be between 3 and 1000.
* Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

### Example
* Input: [[1,2], [1,3], [2,3]]
  * Output: [2,3]. The given undirected graph will be like this:
    ```
      1
     / \
    2 - 3
    ```
* Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
  * Output: [1,4]. The given undirected graph will be like this:
    ```
    5 - 1 - 2
        |   |
        4 - 3
    ```

## Solution 1: Union Find，速度 前1%

### Complexity
* Time: O(n) <==== 因该是多少？？？？
* Space: O(n)

### Java
```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        
        int[] roots = new int[n];
        for (int i = 0; i < n; i++) {
            roots[i] = i;
        }
        
        for (int[] edge : edges) {
            int from = edge[0] - 1, to = edge[1] - 1;
            
            int rootF = findRoot(from, roots);
            int rootT = findRoot(to, roots);
            
            if (rootF == rootT) {
                return new int[]{from + 1, to + 1};
            }
            
            roots[rootF] = to; // 这一句是关键
        }
        return null;
    }
    
    private int findRoot(int node, int[] roots) {
        while(roots[node] != node) {
            node = roots[node];
        }
        return node;
    }
}
```

## Solution 2: 用 Path Compression Weighted Tree 改良过的 Union Find，速度理论上来说应该比上面的大白菜UF要快很多。但是做题的时候用上面的大白菜UF就够了

### Complexity
* Time: O(n) <==== 因该是多少？？？？
* Space: O(n)

### Java
把UF的代码都提出来，放到主体的后面去了。代码看起来有点长，其实不复杂

```java
public class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        PathCompressionWeightedTreeUnionFind uf = new PathCompressionWeightedTreeUnionFind(n);
        
        for (int[] edge : edges) {
            int from = edge[0] - 1, to = edge[1] - 1;
            
            int rootF = uf.getGroupID(from);
            int rootT = uf.getGroupID(to);
            
            if (rootF == rootT) {
                return new int[]{from + 1, to + 1};
            }
            
            uf.union(from, to);
        }
        return null;
    }
    
    private int findRoot(int node, int[] roots) {
        while(roots[node] != node) {
            node = roots[node];
        }
        return node;
    }
}

class PathCompressionWeightedTreeUnionFind {
    private int[] parentIDs;
    private int[] groupSizes;

    public PathCompressionWeightedTreeUnionFind(int n) {
        this.parentIDs = new int[n];
        this.groupSizes = new int[n];
        
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i;
            groupSizes[i] = 1;
        }
    }
    
    public int getGroupID(int index) {
        int curIndex = index;
        
        // step 1, get group ID
        while (curIndex != parentIDs[curIndex]) {
            curIndex = parentIDs[curIndex];
        }
        int groupID = curIndex; // namely the index of root object
        
        // step 2, update the parent id of the object and all its direct ancestors to be
        // the group id, namely the id of the root object
        curIndex = index;
        while (curIndex != groupID) {
            int parentID = parentIDs[curIndex];
            parentIDs[curIndex] = groupID;
            curIndex = parentID;
        }
        return groupID;
    }

    public boolean find(int a, int b) {
        int groupIDA = getGroupID(a);
        int groupIDB = getGroupID(b);
        
        return groupIDA == groupIDB;
    }

    public void union(int a, int b) {
        int groupIDA = getGroupID(a);
        int groupIDB = getGroupID(b);
        
        if (groupIDA == groupIDB) {
            return;
        }
        
        if (groupSizes[groupIDA] >= groupSizes[groupIDB]) {
            parentIDs[groupIDB] = groupIDA;
            groupSizes[groupIDA] += groupSizes[groupIDB];
        } else {
            parentIDs[groupIDA] = groupIDB;
            groupSizes[groupIDB] += groupSizes[groupIDA];
        }
    }
}
```

## Reference
* [Redundant Connection I [LeetCode]](https://leetcode.com/problems/redundant-connection/description/)

{% include links.html %}
