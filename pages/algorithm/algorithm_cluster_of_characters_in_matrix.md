---
title: "Cluster of Characters in Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cluster_of_characters_in_matrix.html
folder: algorithm
toc: false
---

## Description
一个char 矩阵。找里面各个char各自有多少个cluser。连接的方向是八个，不仅是上下左右的4个，斜的4个也算。比如下面这个矩阵：
```
{'a', 'a', 'b', 'c'}
{'a', 'b', 'c', 'c'}
{'c', 'c', 'c', 'a'}
```
返回就是：char: a, number of clusters: 2; char: b, number of clusters: 1; char: c, number of clusters: 1.

### Example
见上文

## Solution：普通的BFS

### Complexity
* Time: O(nm)
* Space: O(1), in place

### Java
```java
public class Solution {
    private static final int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}
    };
    
    public void numOfCharClusters(char[][] matrix) {
        // ignore sanity checks
        
        int n = matrix.length, m = matrix[0].length;
        // <cell char value, number of clusters of this char>
        Map<Character, Integer> map = new HashMap<>();
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                char cur = matrix[i][j];
                if (cur == '-') {
                    continue;
                }
                
                Integer count = map.get(cur);
                if (count == null) count = 0;

                bfs(matrix, i, j);
                count++;
                map.put(cur, count);
            }
        }
        
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            System.out.println("char: " + entry.getKey() + 
                    ", number of clusters: " + entry.getValue());
        }
    }
    
    private void bfs(char[][] matrix, int i, int j) {
        System.out.println("i:"+i+", j:"+j);
        char curChar = matrix[i][j];
        matrix[i][j] = '-'; // mark it as visited
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {i, j});
        
        while (!queue.isEmpty()) {
            int[] curCoord = queue.poll();
            int x = curCoord[0], y = curCoord[1];
            
            for (int[] dir : DIRS) {
                int newX = x + dir[0];
                int newY = y + dir[1];

                if (outOfBound(matrix.length, matrix[0].length, newX, newY)) {
                    continue;
                }
                
                if (matrix[newX][newY] == curChar) {
                    matrix[newX][newY] = '-';
                    queue.offer(new int[] {newX, newY});
                }
            }
        }
    }
    
    private boolean outOfBound(int n, int m, int x, int y) {
        return x < 0 || y < 0 || x >= n || y >= m;
    }
}
```

## Reference
* Uber的电面题。网上没看见这题

{% include links.html %}
