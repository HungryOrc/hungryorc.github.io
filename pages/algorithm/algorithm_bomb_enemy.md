---
title: "Bomb Enemy"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_bomb_enemy.html
folder: algorithm
toc: false
---

## Description
Given a 2D grid, each cell is either a wall 'W', an enemy 'E' or empty '0' (the number zero), return the maximum enemies you can kill using one bomb.

The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.

Note: 
* You can only put the bomb at an empty cell.

### Example
* Input: 
  ```
  0  E  0  0
  E  0  W  E
  0  E  0  0
  ```
  * Output: 3, when placing the bomb at (1, 1)

## Solution：我自己的DP类方法，速度不算快，但思路简明漂亮
Use 4 dp matrix to represent 4 directions of the bomb explosion killing effects.

### Complexity
* Time: O(n * m)
* Space: O(n * m)，DP矩阵的size

### Java
代码看起来较长，其实思路非常直觉简明
```java
class Solution {
    public int maxKilledEnemies(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        int n = grid.length, m = grid[0].length;
        
        // dp[i][j]: 从当前位置 往上/往下/往左/往右 炸，能炸死多少人，不包括当前位置！这么做的因为，
        // 虽然只能往0处投弹，不能往E处投弹，但我们还是要记录投在每个E处时，往上下左右能炸死多少人，
        // 否则，遇到连续多个E的情况，就会丢记录，造成结果偏少
        // 当然每个0处自然也是要记录的
        int[][] goUp = new int[n][m];
        int[][] goDown = new int[n][m];
        int[][] goLeft = new int[n][m];
        int[][] goRight = new int[n][m];
        
        for (int i = 1; i < n; i++) { // 上面第一行不用处理，都留为0，就行了！下面自会处理到这行
            for (int j = 0; j < m; j++) {
                
                // W不管。0和E都要管，特别要注意E！
                if (grid[i][j] == 'W') {
                    continue;
                }
                
                // grid[i - 1][j] 如果是E或0，则可能值 > 0，
                // 如果是W，则值必为0，所以一概加上，都不会错
                goUp[i][j] = goUp[i - 1][j];
                // 如果上一行为E，还得再额外 +1，因为上一行处的计数是不包括上一行本身的！
                if (grid[i - 1][j] == 'E') {
                    goUp[i][j]++;
                }
            }
        }
        
        for (int i = n - 2; i >= 0; i--) { // 最底下一行不用处理，都留为0
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 'W') {
                    continue;
                }

                goDown[i][j] = goDown[i + 1][j];
                if (grid[i + 1][j] == 'E') {
                    goDown[i][j]++;
                }
            }
        }

        for (int j = 1; j < m; j++) { // 最左边一列不用处理，都留为0
            for (int i = 0; i < n; i++) {
                if (grid[i][j] == 'W') {
                    continue;
                }

                goLeft[i][j] = goLeft[i][j - 1];
                if (grid[i][j - 1] == 'E') {
                    goLeft[i][j]++;
                }
            }
        }
        
        for (int j = m - 2; j >= 0; j--) { // 最右边一列不用处理，都留为0
            for (int i = 0; i < n; i++) {
                if (grid[i][j] == 'W') {
                    continue;
                }

                goRight[i][j] = goRight[i][j + 1];
                if (grid[i][j + 1] == 'E') {
                    goRight[i][j]++;
                }
            }
        }
        
        int maxSumOf4Directions = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] != '0') { // 各个E处也是有计数的！但不能管它们！
                    continue;
                }
                
                int sum = goUp[i][j] + goDown[i][j] + goLeft[i][j] + goRight[i][j];
                maxSumOf4Directions = Math.max(maxSumOf4Directions, sum);
            }
        }
        return maxSumOf4Directions;
    }
}
```

## Reference
* [Bomb Enemy [LeetCode]](https://leetcode.com/problems/bomb-enemy/description/)

{% include links.html %}
