---
title: "Number of Islands II: Create Lands by Stream"
tags: [algorithm, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_numberOfIslandsII_createLandsByStream.html
folder: algorithm
toc: false
---

## Description
A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

这题是 Hard 难度

### Example
* Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
  * Output: [1,1,2,3]
  ```
  Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).
  0 0 0
  0 0 0
  0 0 0
  
  Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.
  1 0 0
  0 0 0   Number of islands = 1
  0 0 0
  
  Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.
  1 1 0
  0 0 0   Number of islands = 1
  0 0 0
  
  Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.
  1 1 0
  0 0 1   Number of islands = 2
  0 0 0

  Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.
  1 1 0
  0 0 1   Number of islands = 3
  0 1 0
  ```

## Solution：这题一看就要用 Union Find。我的做法。速度 前20%
这题的主干逻辑都在第一个函数里，其实比较简明。其他的functions都是helpers，所以不要被代码长度吓到了

### Complexity
* Time: O(？？？？？
* Space: O(？？？？？

### Java
```java
class Solution {
    private static final int[][] DIRS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    private static final int MULTIPLIER = 100000;
    
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        List<Integer> result = new ArrayList<>();
        if (m <= 0 || n <= 0 || 
            positions == null || positions.length == 0 || positions[0].length == 0) {
            return result;
        }
        
        int[][] matrix = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                matrix[i][j] = -1;
            }
        }
        
        int[] count = {0};
        
        for (int i = 0; i < positions.length; i++) {
            int curX = positions[i][0];
            int curY = positions[i][1];
            
            matrix[curX][curY] = getID(m, curX, curY);
            count[0]++;
            
            for (int[] dir : DIRS) {
                int x = curX + dir[0];
                int y = curY + dir[1];
                
                if (!validCoord(m, n, x, y)) {
                    continue;
                }
                
                if (matrix[x][y] != -1) {
                    union(matrix, curX, curY, x, y, count);
                }
            }
            
            int curCount = count[0];
            result.add(curCount);
        }
        return result;
    }
    
    // ---------------------------- 下面都是 helper functions 了 -------------------------------
    
    private void union(int[][] matrix, int x1, int y1, int x2, int y2, int[] count) {
        int groupID1 = getGroupID(matrix, x1, y1);
        int groupID2 = getGroupID(matrix, x2, y2);
        
        if (groupID1 == groupID2) {
            return;
        }
        
        count[0]--;
        
        int[] coordOfRoot1 = parseID(matrix.length, groupID1);
        matrix[coordOfRoot1[0]][coordOfRoot1[1]] = groupID2;
    }
    
    private int getGroupID(int[][] matrix, int x, int y) {
        int m = matrix.length;
        int originX = x, originY = y;
        
        // find the group ID
        int parentID = matrix[x][y];
        while (parentID != getID(m, x, y)) {
            int[] coord = parseID(m, parentID);
            x = coord[0];
            y = coord[1];
            parentID = matrix[x][y];
        }
        int groupID = parentID;
        
        // set the parent ID for (x, y) and (x, y)'s ancestors to be the group ID
        x = originX;
        y = originY;
        while (matrix[x][y] != groupID) {
            parentID = matrix[x][y];
            matrix[x][y] = groupID;
            
            int[] coord = parseID(m, parentID);
            x = coord[0];
            y = coord[1];
        }
        
        return groupID;
    }
    
    private int getID(int m, int x, int y) {
        // 如果不乘以这个乘数，那么如果列数比行数大，比如m=3，n=10的时候，除法和mod就无法确保得到正确的行列数
        return x * m * MULTIPLIER + y; 
    }
    
    private int[] parseID(int m, int id) {
        int[] coord = new int[2];
        coord[0] = id / (m * MULTIPLIER);
        coord[1] = id % (m * MULTIPLIER);
        return coord;
    }
    
    private boolean validCoord(int m, int n, int x, int y) {
        return (x >= 0 && x < m && y >= 0 && y < n);
    }
}
```

## Reference
* [Number of Islands II [LeetCode]](https://leetcode.com/problems/number-of-islands-ii/description/)

{% include links.html %}
