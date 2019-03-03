---
title: "Number of Islands"
tags: [algorithm, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_numberOfIslands.html
folder: algorithm
toc: false
---

## Description
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Example
* Input:
  ```
  11110
  11010
  11000
  00000
  ```
  * Output: 1
* Input: 
  ```
  11000
  11000
  00100
  00011
  ```
  * Output: 3

## Solution 1: BFS

### Complexity
* Time: O(m*n)
* Space: O(m + n)，我认为最坏情况是bfs的queue装满了整个grid的最外一圈，这对么 ？？？

### Java
```java
public class Solution {
  private static final int[][] DIRS = {
    {1,0}, {-1,0}, {0,1}, {0,-1}
  };
  
  public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
      return 0;
    }
    
    int numOfIslands = 0;
    int m = grid.length;
    int n = grid[0].length;
    
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (grid[i][j] == '1') {
          paintZero(grid, i, j);
          numOfIslands ++;
        }
      }
    }
    
    return numOfIslands;
  }
  
  private void paintZero(char[][] grid, int x, int y) {
    Queue<Integer[]> coords = new LinkedList<>();
    coords.offer(new Integer[]{x, y});
    
    while (!coords.isEmpty()) {
      Integer[] curCoord = coords.poll();
      int curX = curCoord[0];
      int curY = curCoord[1];
      grid[curX][curY] = '0';
      
      for (int d = 0; d < 4; d++) {
        int newX = curX + DIRS[d][0];
        int newY = curY + DIRS[d][1];
        if (isValid(newX, newY, grid.length, grid[0].length) && grid[newX][newY] == '1') {
          grid[newX][newY] = '0';
          coords.offer(new Integer[]{newX, newY});
        }
      }
    }
  }
  
  private boolean isValid(int x, int y, int m, int n) {
    return (x >= 0 && x < m) && (y >= 0 && y < n);
  }
}
```

## Solution 2: DFS，自己写一下！下面是九章的答案！必须自己写一次！

### Complexity
* Time: O(m*n)
* Space: O(m*n)，最坏的情况是一次dfs就把整个grid里的元素都搞进去了

### Java
```java
public class Solution {
    private int m, n;
    
    public int numIslands(boolean[][] grid) {
        m = grid.length;
        if (m == 0) return 0;
        n = grid[0].length;
        if (n == 0) return 0;
        
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == false) { 
                    continue;
                } else { // grid[i][j] == true
                    result ++;
                    dfs(grid, i, j);
                }
            }
        }
        return result;
    }
    
    public void dfs(boolean[][] grid, int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n) 
            return;
        
        if (grid[i][j] == true) {
            grid[i][j] = false;
            dfs(grid, i - 1, j);
            dfs(grid, i + 1, j);
            dfs(grid, i, j - 1);
            dfs(grid, i, j + 1);
        }
    }
}
```

## Solution 3: Union Find，自己写一下！必须自己写一次！

### Complexity
* Time: O(m*n)
* Space: O(m*n)

### Java
```java







```

## Reference
* [Number of Islands [LeetCode]](https://leetcode.com/problems/number-of-islands/description/)

{% include links.html %}
