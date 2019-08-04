---
title: "Min Transformed Values on a Grid"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_minTransformedValues_onGrid.html
folder: algorithm
toc: false
---

## Description
把一个int grid 转化成一个transformed grid，这个新的grid里的每个数都必须是正整数，且能体现原grid里的数的大小关系，且新grid里的最大值要尽可能地小

比如给一个grid：
```
10  50  70  30  40
-20  0  30  0  20
90  10  10  10  -50
```
得到的结果应该是：
```
2  6  7  4  5
1  2  4  2  3
4  3  3  3  1
```

### Example
如上

## Solution
我自己的方法。思路有些难想，但也不是特别难。要细心。关键点有2个：
* 如何判断一个cell已经彻底安全了，即它的值不再需要增大了，即它所在的行和列的所有其他cell都不会促使它再增大了
* 在处理一个cell的时候，如何处理与它同行（或同列）的其它cells，如果那些cell目前还没被赋值怎么办，如果比当前cell大怎么办，如果比当前cell小怎么办

参考：我归纳的另一题：仅需要transform一个array的那一题

要注意：原数组里可能有重复的值

### Complexity
* Time: O(n^2 * m^2 ？？？？) <==== 这个分析就有点复杂了，该是多少 ？？？？
* Space: O(n * m)

### Java
代码里的print函数和statements都是为了测试，main函数也是为了测试，可以忽略它们

```java
class Cell implements Comparable<Cell> {
    int value;
    int x, y;
    
    public Cell(int v, int x, int y) {
        this.value = v;
        this.x = x;
        this.y = y;
    }
    
    public int compareTo(Cell c2) {
        return Integer.compare(this.value, c2.value);
    }
}

public class Solution {
    
    public int[][] transformGrid(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        
        int[][] transGridByRows = getTransferredGrid(grid, true);
        System.out.println("Grid of transformed values by rows: ");
        printGrid(transGridByRows);
        int[][] transGridByCols = getTransferredGrid(grid, false);
        System.out.println("Grid of transformed values by col: ");
        printGrid(transGridByCols);
        
        int[][] result = new int[n][m];
        boolean[][] settled = new boolean[n][m];
        
        List<Cell> smallestCells = getSmallestCellsInGrid(grid);
        
        PriorityQueue<Cell> minHeap = new PriorityQueue<>();
        for (Cell c : smallestCells) {
            int x = c.x, y = c.y;
            result[x][y] = c.value;
            markSettled(settled, x, y);
            minHeap.offer(c);
        }
        
        while (!minHeap.isEmpty()) {
            Cell curCell = minHeap.poll();
            
            // 这一题的关键，是下面的2个函数
            
            // 此函数的主要目的，是确保 当前cell的value 要 >= 当前row里的 应该<=它的cells的values，
            // 且当前cell的value尽可能地小。
            // 次要目的是在当前row的空cell里，填入暂时的值。
            // 注意，本函数不改变 当前row里的任何 已经有value的且不是当前cell的cell
            manageItsRow(curCell, transGridByRows, result, settled, minHeap);
            System.out.println(
                    "Result grid after managed by row for cell (" + curCell.x + ", " + curCell.y + "):");
            printGrid(result);
            
            // 此函数的主要目的，是确保 当前cell的value 要 >= 当前col里的 应该<=它的cells的values，
            // 且当前cell的value尽可能地小。
            // 次要目的是在当前col的空cell里，填入暂时的值。
            // 注意，本函数不改变 当前col里的任何 已经有value的且不是当前cell的cell    
            manageItsCol(curCell, transGridByCols, result, settled, minHeap);
            System.out.println(
                    "Result grid after managed by col for cell (" + curCell.x + ", " + curCell.y + "):");
            printGrid(result);
            
            if (!cellIsSettled(curCell, transGridByRows, transGridByCols, settled)) {
                minHeap.offer(curCell);
            }
        }
        return result;
    }

    private int[] transformArray(int[] nums) {
        int n = nums.length; 
        
        int[] sortedNums = new int[n];
        for (int i = 0; i < n; i++) {
            sortedNums[i] = nums[i];
        }
        
        // 不要直接从grid里搞一个row出来放这里！否则grid里的那个row会被改变的！
        Arrays.sort(sortedNums);
        
        // <value, index in the sorted array>
        Map<Integer, Integer> map = new HashMap<>();
        map.put(sortedNums[0], 1);
        int count = 1;
        for (int i = 1; i < n; i++) {
            if (sortedNums[i] > sortedNums[i - 1]) {
                count++;
                map.put(sortedNums[i], count);
            }
        }
        
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = map.get(nums[i]);
        }
        return result;
    }

    private void markSettled(boolean[][] settled, int x, int y) {
        settled[x][y] = true;
        System.out.println("Cell (" + x + ", " + y + ") is now settled!\n");
    }
    
    private int[][] getTransferredGrid(int[][] grid, boolean byRow) {
        int n = grid.length;
        int m = grid[0].length;
        int[][] result = new int[n][m];
        
        if (byRow) {
            for (int i = 0; i < n; i++) {
                int[] row = grid[i];
                result[i] = transformArray(row);
            }
        } else { // if by column
            for (int j = 0; j < m; j++) {
                int[] col = new int[n];
                for (int i = 0; i < n; i++) {
                    col[i] = grid[i][j];
                }

                int[] transformedCol = transformArray(col);
                for (int i = 0; i < n; i++) {
                    result[i][j] = transformedCol[i];
                }
            }
        }
        return result;
    }
    
    private List<Cell> getSmallestCellsInGrid(int[][] grid) {
        int min = Integer.MAX_VALUE;
        List<Cell> result = new ArrayList<>();
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] < min) {
                    min = grid[i][j];
                    result = new ArrayList<>();
                    result.add(new Cell(min, i, j));
                } else if (grid[i][j] == min) {
                    result.add(new Cell(min, i, j));
                }
            }
        }
        return result;
    }
    
    private void manageItsRow(Cell cell, 
            int[][] transGridByRows, int[][] result, boolean[][] settled, PriorityQueue<Cell> minHeap) {
        int curX = cell.x, curY = cell.y;
        int transValueOfCurCell = transGridByRows[curX][curY];
        result[curX][curY] = Math.max(result[curX][curY], transValueOfCurCell);

        for (int i = 0; i < result[0].length; i++) {
            if (i == curY) continue;
            if (result[curX][i] == 0) {
                result[curX][i] = transGridByRows[curX][i];
                minHeap.offer(new Cell(transGridByRows[curX][i], curX, i));
            }
            // 下面这个 if block 是关键所在
            if (transGridByRows[curX][i] <= transValueOfCurCell) {
                result[curX][curY] = Math.max(
                        result[curX][curY], 
                        result[curX][i] + (transValueOfCurCell - transGridByRows[curX][i]));
            }
        }
    }

    private void manageItsCol(Cell cell, 
            int[][] transGridByCols, int[][] result, boolean[][] settled, PriorityQueue<Cell> minHeap) {
        int curX = cell.x, curY = cell.y;
        int transValueOfCurCell = transGridByCols[curX][curY];
        result[curX][curY] = Math.max(result[curX][curY], transValueOfCurCell);

        for (int i = 0; i < result.length; i++) {
            if (i == curX) continue;
            if (result[i][curY] == 0) {
                result[i][curY] = transGridByCols[i][curY];
                minHeap.offer(new Cell(transGridByCols[i][curY], i, curY));
            }
            // 下面这个 if block 是关键所在
            if (transGridByCols[curX][i] <= transValueOfCurCell) {
                result[curX][curY] = Math.max(
                        result[curX][curY], 
                        result[i][curY] + (transValueOfCurCell - transGridByCols[i][curY]));
            }
        }
    }
    
    private boolean cellIsSettled(Cell cell, 
            int[][] transGridByRows, int[][] transGridByCols, boolean[][] settled) {
        int x = cell.x, y = cell.y;
        int transValueByRowForCurCell = transGridByRows[x][y];
        int transValueByColForCurCell = transGridByCols[x][y];
        
        for (int j = 0; j < settled[0].length; j++) {
            if (transGridByRows[x][j] < transValueByRowForCurCell) {
                if (!settled[x][j]) return false;
            }
        }
        for (int i = 0; i < settled.length; i++) {
            if (transGridByCols[i][y] < transValueByColForCurCell) {
                if (!settled[i][y]) return false;
            }
        }
        
        markSettled(settled, x, y);
        return true;
    }
    
    // ------------------- main function and print function -------------------
    public static void main(String[] args) {
        int[][] grid = new int[][] {
            {10, 50, 70, 30, 40}, 
            {-20, 0, 30, 0, 20}, 
            {90, 10, 10, 10, -50}
        };
        System.out.println("Original grid is: ");
        printGrid(grid);
        
        Solution solu = new Solution();
        int[][] result = solu.transformGrid(grid);
        System.out.println("Final result is: ");
        printGrid(result);
    }
    
    private static void printGrid(int[][] grid) {
        for (int[] row : grid) {
            for (int num : row) System.out.print(num + "  ");
            System.out.println();
        }
        System.out.println();
    }
}
```

## Reference
* 我自己被Waymo电面的题目，没在网上看到这题

{% include links.html %}
