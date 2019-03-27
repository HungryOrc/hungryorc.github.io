---
title: "Island Perimeter 周长"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_island_perimeter.html
folder: algorithm
toc: false
---

## Description
You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. 
Grid cells are connected horizontally/vertically (not diagonally). 
The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). 
The island doesn't have "lakes" (water inside that isn't connected to the water around the island). 
One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. 
Determine the perimeter of the island.

### Example
* Input: 
  ```
  [0,1,0,0]
  [1,1,1,0]
  [0,1,0,0]
  [1,1,0,0]
  ```
  * Output: 16

## Solution 1: 很巧妙, for each island cell, -1 if it has a rightwards/downwards neighbor
Ref: https://discuss.leetcode.com/topic/68786/clear-and-easy-java-solution

从左到右，从上到下，一格一格地看，记录下island cells（即等于1的cell）的个数，
同时记录下每个island cell右边是不是有紧邻的island cell，有则总邻居记录 + 1；
下方是不是有紧邻的 island cell，有则总邻居记录 + 1

每有1个邻居则意味着有2个 island cell 各需要减去一个边，即有2个边不能再算在周长里了。
注意！只考察下方和右边的邻居，是为了让邻居关系不被重复计算！

所以最后 周长 = island cell个数 * 4 - 邻居个数 * 2

### Complexity
* Time: O(n^2)
* Space: O(1)

### Java
```java
public class Solution {
    public int islandPerimeter(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;

        int islands = 0;
        int neighbors = 0;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 1) { // this cell is an island
                    islands ++;

                    // 如果本cell的下方有紧贴的土地
                    if (i < rows - 1 && grid[i + 1][j] == 1) {
                        neighbors ++;
                    }
                    // 如果本cell的右边有紧贴的土地
                    if (j < cols - 1 && grid[i][j + 1] == 1) {
                        neighbors ++;
                    }
                }
            }
        }
        return islands * 4 - neighbors * 2;
    }
```

## Reference
* [Island Perimeter [LeetCode]](https://leetcode.com/problems/island-perimeter/)

{% include links.html %}
