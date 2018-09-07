---
title: "Max Area of Island"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_max_area_of_island.html
folder: algorithm
toc: false
---

## Description
Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

### Example
略

## Solution
DFS，2层 for loop 然后 recursion

### Complexity
* Time: O(n * m)
* Space: O(n * m), each layer of call stack uses O(1) space

### Java
```java
class Solution {
    private static final int[][] DIRS = {
        {1,0}, {-1,0}, {0,1}, {0,-1}
    };
    
    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        int maxArea = 0; // 注意这里及下面的写法！根本不用设maxArea为全局变量！
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    maxArea = Math.max(maxArea, findMax(grid, i, j));
                }
            }
        }
        
        return maxArea;
    }
    
    private int findMax(int[][] grid, int x, int y) {
        if (x < 0 || y < 0 || x >= grid.length || y >= grid[0].length) {
            return 0;
        }

        int curArea = 0; // 注意这个写法！curArea不一定要搞成一个长度为1的int数组然后跟着每层recursion传递
        if (grid[x][y] == 1) {
            grid[x][y] = 0;
            curArea = 1;
            
            for (int[] dir : DIRS) {
                curArea += findMax(grid, x + dir[0], y + dir[1]);
            }
        }
        return curArea;
    }
}
```

## Reference
* [Max Area of Island [LeetCode]](https://leetcode.com/problems/max-area-of-island/description/)

{% include links.html %}
