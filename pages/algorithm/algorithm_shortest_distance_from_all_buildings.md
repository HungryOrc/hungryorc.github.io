---
title: "Shortest Distance from All Buildings"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortest_distance_from_all_buildings.html
folder: algorithm
toc: false
---

## Description
You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:
* Each 0 marks an empty land which you can pass by freely.
* Each 1 marks a building which you cannot pass through.
* Each 2 marks an obstacle which you cannot pass through.

There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

这题leetcode上标的是hard题，其实只是median难度

### Example
* Input: [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]
  * Output: 7. Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2), the point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

## Solution 1: 从空地出发，找各个房子。这个方法速度非常慢！因为空地很多

### Complexity
* Time: O(空地的个数 x n x m)
* Space: O(n x m)

### Java
代码看起来不短，思路其实很简明
```java
class Solution {
    private final static int EMPTY = 0;
    private final static int BUILDING = 1;
    private final static int OBSTACLE = 2;
    
    private final static int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}
    };
    
    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        
        int n = grid.length;
        int m = grid[0].length;
        int countBldg = 0;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == BUILDING) {
                    countBldg++;
                }
            }
        }
        if (countBldg == 0) return -1;
        
        int minDists = Integer.MAX_VALUE;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] != EMPTY) {
                    continue;
                }
                
                int dists = 0;
                int step = 0;
                int reached = 0;
                
                Integer[] original = new Integer[]{i, j};
                boolean[][] visited = new boolean[n][m];
                Queue<Integer[]> queue = new LinkedList<>();
                queue.offer(original);
                
                while (!queue.isEmpty()) {
                    step++;
                    int size = queue.size(); // 这个千万不能浮动！不能放在下面！必须放在这里！
                    
                    for (int k = 0; k < size; k++) {
                        Integer[] curCoord = queue.poll();
                        int curX = curCoord[0];
                        int curY = curCoord[1];
                        
                        for (int[] dir : DIRS) {
                            int x = curX + dir[0];
                            int y = curY + dir[1];
                            Integer[] coord = new Integer[]{x, y};
                            
                            if (!inBound(grid, x, y) || visited[x][y] || grid[x][y] == OBSTACLE) {
                                continue;
                            }
                            
                            if (grid[x][y] == BUILDING) {
                                dists += step;
                                reached++;
                            }
                            
                            if (grid[x][y] == EMPTY && !visited[x][y]) {
                                queue.offer(coord);
                            }
                            
                            visited[x][y] = true;
                        }
                    }
                }
                
                if (reached == countBldg) {
                    minDists = Math.min(minDists, dists);
                }
            }
        }
        
        if (minDists == Integer.MAX_VALUE) {
            return -1;
        }
        return minDists;
    }
    
    private boolean inBound(int[][] grid, int x, int y) {
        return x >= 0 && x < grid.length && y >= 0 && y < grid[0].length;
    }
}
```

## Solution 2: 从房子出发，找各个空地。这个方法比上面的方法快很多，因为一般来说房子少空地多
代码也比上面的稍微短一点

### Complexity
* Time: O(房子的个数 x n x m)
* Space: O(n x m)

### Java
代码看起来不短，思路其实很简明
```java
class Solution {
    private final static int EMPTY = 0;
    private final static int BUILDING = 1;
    private final static int OBSTACLE = 2;
    
    private final static int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}
    };
    
    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        
        int n = grid.length;
        int m = grid[0].length;
        int countBldg = 0;
                        
        int[][] visitCounts = new int[n][m];
        int[][] dists = new int[n][m];
        
        int minDists = Integer.MAX_VALUE;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] != BUILDING) {
                    continue;
                }
                countBldg++;
                
                int step = 0;
                Integer[] original = new Integer[]{i, j};
                
                boolean[][] visited = new boolean[n][m];
                visited[i][j] = true;
                
                Queue<Integer[]> queue = new LinkedList<>();
                queue.offer(original);
                
                while (!queue.isEmpty()) {
                    step++;
                    int size = queue.size(); // 这个千万不能浮动！不能放在下面！必须放在这里！
                    
                    for (int k = 0; k < size; k++) {
                        Integer[] curCoord = queue.poll();
                        int curX = curCoord[0];
                        int curY = curCoord[1];
                        
                        for (int[] dir : DIRS) {
                            int x = curX + dir[0];
                            int y = curY + dir[1];
                            Integer[] coord = new Integer[]{x, y};
                            
                            if (!inBound(grid, x, y) || visited[x][y] || grid[x][y] == OBSTACLE) {
                                continue;
                            }
                            
                            if (grid[x][y] == EMPTY) {
                                dists[x][y] += step;
                                visitCounts[x][y]++;
                                queue.offer(coord);
                            }
                            
                            visited[x][y] = true;
                        }
                    }
                }
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (visitCounts[i][j] == countBldg) {
                    minDists = Math.min(minDists, dists[i][j]);
                }
            }
        }
        
        if (minDists == Integer.MAX_VALUE) {
            return -1;
        }
        return minDists;
    }
    
    private boolean inBound(int[][] grid, int x, int y) {
        return x >= 0 && x < grid.length && y >= 0 && y < grid[0].length;
    }
}
```

## Reference
* [Shortest Distance from All Buildings [LeetCode]](https://leetcode.com/problems/shortest-distance-from-all-buildings/description/)

{% include links.html %}
