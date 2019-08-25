---
title: "Sum of Distances in Tree"
tags: [algorithm, bfs]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_sum_of_distances_in_tree.html
folder: algorithm
toc: false
---

## Description: Hard，但也不是那么难
An undirected, connected tree with N nodes labelled 0...N-1 and N-1 edges are given. The ith edge connects nodes edges[i][0] and edges[i][1] together.

Return a list ans, where ans[i] is the sum of the distances between node i and all other nodes.

Note:
* 1 <= N <= 10000

### Example
* Input: N = 6, edges = `[[0,1],[0,2],[2,3],[2,4],[2,5]]`
  ```
    0
   / \
  1   2
     /|\
    3 4 5
  ```
  * Output: [8,12,6,10,10,10]. We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) equals 1 + 1 + 2 + 2 + 2 = 8.  Hence, answer[0] = 8, and so on.

## Solution 1: 对于图里的每一个点，以它作为整个树的根，做一次BFS。速度慢

### Complexity
* Time: O(|V| * (|V| + |E|)) <==== 我觉得应该是 O(|V| * |E|) ？？？？
* Space: O(traversal ways)，因为这是BFS的方法

### Java
```java
class Solution {
    public int[] sumOfDistancesInTree(int n, int[][] edges) {
        int[] result = new int[n];
        
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(i, new ArrayList<>());
        }
        for (int[] edge : edges) {
            int start = edge[0], end = edge[1];
            List<Integer> sList = map.get(start);
            sList.add(end);
            List<Integer> eList = map.get(end);
            eList.add(start);
        }
        
        // 对每一个node做一次BFS
        for (int i = 0; i < n; i++) {
            boolean[] visited = new boolean[n];
            visited[i] = true;
            
            int sum = 0;
            int dist = 0;
            
            Queue<Integer> queue = new LinkedList<>();
            queue.offer(i);
            
            while (!queue.isEmpty()) {
                int size = queue.size(); // 这个一定要先算出来！固定好！我因为这个出bug好几次了！
                
                for (int j = 0; j < size; j++) {
                    int curNode = queue.poll();
                    sum += dist;
                    
                    for (int nextNode : map.get(curNode)) {
                        if (visited[nextNode]) continue;
                        
                        visited[nextNode] = true;
                        queue.offer(nextNode);
                    }
                }
                dist++;
            }
            result[i] = sum;
        }
        return result;
    }
}
```

## Solution 2: 数学的方法。非常巧妙。很难想到。要透彻理解
这个视频讲解得很清楚：https://www.youtube.com/watch?v=gi2maECPOB0

### Complexity
* Time: O(|V| + |E|) <==== ？？？？
* Space: O(|V| + |E|)，map 的 size <==== ？？？？

### Java
```java

```

## Reference
* [Sum of Distances in Tree [LeetCode]](https://leetcode.com/problems/sum-of-distances-in-tree/description/)

{% include links.html %}
