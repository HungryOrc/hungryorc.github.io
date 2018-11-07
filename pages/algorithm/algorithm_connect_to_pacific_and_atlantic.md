---
title: "Connect to both Pacific and Atlantic"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_connect_to_pacific_and_atlantic.html
folder: algorithm
toc: false
---

## Description
Given an m*n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

* The order of returned grid coordinates does not matter.
* Both m and n are less than 150.

### Example
* Input: 
  ```
  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic
  ```
  * Output: positions with parentheses in above matrix:
    * [[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]]

## Solution 1: 从多个点开始出发的 BFS
所谓的 KBFS

先把周围的边标上true。然后从这些边上的点们出发，往矩阵的中间扩散。

特别要注意的一点！我在这里写错了：**不应该设置visited这么一个helper矩阵！**
* 因为对于原matrix里的一个cell来说，从一个路径access它的时候，它可能还不能与某个大洋相连，但之后从另一个路径再次access它的时候，它说不定就可以和这个
大洋相连了！
* 如果设置了visited这么一个helper矩阵，那么一次access之后，不论结果如何，就断绝了这个cell再次被评判的机会！这样是不对的

### Complexity
* Time: O(n*m)
* Space: O(n*m)

### Java
```java
class Solution {
    private static final int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}
    };
    
    public List<int[]> pacificAtlantic(int[][] matrix) {
        List<int[]> result = new ArrayList<>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return result;
        }

        int n = matrix.length;
        int m = matrix[0].length;
        
        // step 1: deal with pacific
        boolean[][] pac = new boolean[n][m];
        Queue<int[]> queue = new LinkedList<>();
        
        for (int i = 0; i < n; i++) {
            pac[i][0] = true;
            queue.offer(new int[]{i, 0});
        }
        for (int j = 0; j < m; j++) {
            pac[0][j] = true;
            queue.offer(new int[]{0, j});
        }
        
        bfs(matrix, pac, queue);

        // step 2: deal with atlantic
        boolean[][] atl = new boolean[n][m];
        queue = new LinkedList<>(); // reset to reuse
        
        for (int i = 0; i < n; i++) {
            atl[i][m - 1] = true;
            queue.offer(new int[]{i, m - 1});
        }
        for (int j = 0; j < m; j++) {
            atl[n - 1][j] = true;
            queue.offer(new int[]{n - 1, j});
        }
        
        bfs(matrix, atl, queue);   
        
        // step 3: get final result
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (pac[i][j] && atl[i][j]) {
                    result.add(new int[]{i, j});
                }
            }
        }
        return result;
    }
    
    private void bfs(int[][] matrix, boolean[][] sea, Queue<int[]> queue) {
        while (!queue.isEmpty()) {
            int[] curCell = queue.poll();
            int curX = curCell[0], curY = curCell[1];
            
            for (int i = 0; i < 4; i++) {
                int x = curX + DIRS[i][0];
                int y = curY + DIRS[i][1];
                if (isValid(x, y, matrix.length, matrix[0].length) && !sea[x][y]) {
                    if (matrix[x][y] >= matrix[curX][curY]) {
                        sea[x][y] = true;
                        queue.offer(new int[]{x, y});
                    }
                }
            }
        }
    }
    
    private boolean isValid(int x, int y, int n, int m) {
        return (x >= 0 && x < n) && (y >= 0 && y < m);
    }
}
```

## Reference
* [Pacific Atlantic Water Flow [LeetCode]](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

{% include links.html %}
