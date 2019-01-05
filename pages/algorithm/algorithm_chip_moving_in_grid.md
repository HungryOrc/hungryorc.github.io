---
title: "Chip Moving in Grid"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_chip_moving_in_grid.html
folder: algorithm
toc: false
---

## Description：这题有点难
You are given a grid filled with non-negative integer numbers, and a chip is placed on the top left cell. 
On each turn the chip can be moved either to the right neighboring cell or to the bottom neighboring cell. 
The cost of the move is equal to the number written on the resulting cell.
There is also an added cost if you change direction between two consecutive moves. 
In order to change the direction you'll need to pay an additional cost of 10.

What is the minimum amount of points needed to reach the right bottom corner of the grid?
Assume that you don't pay for standing on the upper left cell in the beginning, so the number on that cell is not used.
It is guaranteed that each of all possible paths will cost less than 10^7 points.
* 1 ≤ grid.length ≤ 10,
* 1 ≤ grid[0].length ≤ 10,
* 0 ≤ grid[i][j] ≤ 2000.

### Example
* Input: 给的 grid 如下：
  ```
  [ 0,  0, 99, 99, 99]
  [99,  0,  0,  0, 99
  [99, 99, 99,  0, 99]
  [99, 99, 99,  0, 99]
  [99, 99, 99,  0,  0]
  ```
  * Output: 40. You could avoid all the 99 spots but you would need to change direction 4 times

## Solution：我的方法，使用 2个 二维DP矩阵，各表征一个“进入的方向”
* 一个DP矩阵记录从grid左上角开始，经过若干步，最近的一步是从当前cell的左邻cell到达当前cell，这种情况下的最小cost（含当前cell的值）
* 一个DP矩阵记录从grid左上角开始，经过若干步，最近的一步是从当前cell的上邻cell到达当前cell，这种情况下的最小cost（含当前cell的值）
* 最后的结果就是这2个DP矩阵在grid的右下角处，哪个矩阵在那一个cell的值小，就取谁

### Complexity
* Time: O(n * m)，其中n和m是本题给的矩阵的行数和列数
* Space: O(n * m)

### Java
```java
public int chipMoving(int[][] grid, int directChangeCost) {
    // ignore validations
    
    int n = grid.length;
    int m = grid[0].length;
    
    // 定义两个DP矩阵，表征两个不同的“进入方向”
    int[][] reachCurCellFromAbove = new int[n][m];
    int[][] reachCurCellFromLeft = new int[n][m];
    
    // 第一行第一列即左上角的 [0, 0] cell，
    // 它是起始点，题目说了占据它不产生任何cost
    
    // 第一行 除了[0, 0] 的cells
    // 从左边进入它们只有一种方式，就是从左边的邻cell直接过来，之前不可能有任何转弯
    // 从上方进入它们是不可能的
    for (int col = 1; col < m; col++) {
        reachCurCellFromLeft[0][col] = reachCurCellFromLeft[0][col - 1] + grid[0][col];
    }
    // 第一列 除了[0, 0] 的cells
    // 从上方进入他们只有一种方式，就是从上方的邻cell直接过来，之前不可能有任何转弯
    // 从左方进入它们是不可能的
    for (int row = 1; row < n; row++) {
        reachCurCellFromAbove[row][0] = rachCurCellFromAbove[row - 1][0] + grid[row][0];
    }
    
    // 第二行里 除了第一列 的cells
    // 从上方进入它们只有一种情况：它上方的邻居nei只可能从左边被进入(因为nei在第一行)，所以从nei到它必须转一次弯
    for (int col = 1; col < m; col++) {
        reachCurCellFromAbove[1][col] = reachCurCellFromLeft[0][col] + 
                                        directChangeCost + grid[1][col];
    }
    // 第二列里 除了第一行 的cells
    // 从左边进入它们只有一种情况：它左边的邻居nei只可能从上方被进入(因为nei在第一列)，所以从nei到它必须转一次弯
    for (int row = 1; row < n; row++) {
        reachCurCellFromLeft[row][1] = reachCurCellFromAbove[row][0] + 
                                       directChangeCost + grid[row][1];
    }
    // 第二行里 除了第一二列 的cells
    // 从左边进入它们有两种情况：
    // 要么它的左邻是从左边被进入的，这样从左邻到它就没拐弯；要么它的左邻是从上方被进入的，这样从左邻到它就有拐弯
    for (int col = 2; col < m; col++) {
        reachCurCellFromLeft[1][col] = grid[1][col] + 
            Math.min(reachCurCellFromLeft[1][col - 1], 
                     reachCurCellFromAbove[1][col - 1] + directChangeCost);
    }
    // 第二列里 除了第一二行 的cells
    // 从上方进入它们有两种情况：
    // 要么它的上邻是从上方被进入的，这样从上邻到它就没拐弯；要么它的上邻是从左方被进入的，这样从上邻到它就有拐弯
    for (int row = 2; row < n; row++) {
        reachCurCellFromAbove[row][1] = grid[row][1] + 
            Math.min(reachCurCellFromAbove[row - 1][1], 
                     reachCurCellFromLeft[row - 1][1] + directChangeCost);
    }
    
    // 对于所有第三行(含)第三列(含)以后的cells，如下，它们在两个方向都会有两种情况了
    for (int row = 2; row < n; row++) {
        for (int col = 2; col < m; col++) {
            reachCurCellFromAbove[row][col] = grid[row][col] + 
                Math.min(reachCurCellFromAbove[row - 1][col], 
                         reachCurCellFromLeft[row - 1][col] + directChangeCost);
            reachCurCellFromLeft[row][col] = grid[row][col] + 
                Math.min(reachCurCellFromLeft[row][col - 1], 
                         reachCurCellFromAbove[row][col - 1] + directChangeCost);
        }
    }

    return Math.min(reachCurCellFromAbove[rows - 1][cols - 1], 
                    reachCurCellFromLeft[rows - 1][cols - 1]);   
}
```

## Reference
这题好像在 codeFights 网站上有

{% include links.html %}
