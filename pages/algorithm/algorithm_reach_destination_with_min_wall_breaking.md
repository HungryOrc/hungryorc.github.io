---
title: "Shortest Path in a Matrix with Max Wall Breaking Counts"
tags: [algorithm]
keywords: wall, obstacle, break
summary:
sidebar: mydoc_sidebar
permalink: algorithm_reach_destination_with_min_wall_breaking.html
folder: algorithm
toc: false
---

## Description
一个n行m列的迷宫，里面1的cell是墙，0的cell是路。每次可以走上下左右四个方向。起始点是左上角，终点是右下角。
允许破墙无数次，所以最后一定能到达终点。
问最少破墙几次能到终点。

### Example
* Input:
  ```
  {1, 1, 1, 1, 0},
  {0, 1, 0, 1, 1},
  {0, 1, 0, 1, 1}
  ```
  * Output: 4, means we can reach the destination with minimum 4 wall breakings
  * 每破一次墙能到达的各个 0 cell 和 1 cell 如下：
    ```
    '0' Cells reachable after break wall 0 times:
    '1' Cells reachable after break wall 1 times:
        x: 0, y: 0
    '0' Cells reachable after break wall 1 times:
        x: 1, y: 0
        x: 2, y: 0
    '1' Cells reachable after break wall 2 times:
        x: 0, y: 1
        x: 1, y: 1
        x: 2, y: 1
    '0' Cells reachable after break wall 2 times:
        x: 1, y: 2
        x: 2, y: 2
    '1' Cells reachable after break wall 3 times:
        x: 0, y: 2
        x: 1, y: 3
        x: 2, y: 3
    '0' Cells reachable after break wall 3 times:
    '1' Cells reachable after break wall 4 times:
        x: 0, y: 3
        x: 1, y: 4
        x: 2, y: 4
    ```

## Solution
注意，因为只关心破墙次数，所以每一次破墙之前走了多少步 0 cell 都没影响！
所以这不是一个典型的最短路径问题，而是一个**“最少破墙”**问题。
我们需要用一种新的**“分层”**的思路来解决这类问题。如下图：
```
0始 0零 1一 1二 1二 0一 0一 0一
0零 0零 1一 0一 0一 0一 0一 0一
0零 0零 1一 1二 1二 1二 1二 1二
0零 0零 1一 1二 0二 0二 0二 0二
0零 0零 1一 1二 0二 0二 0二 0终

```

从0往外找0和1的时候，遇到的0都装到q0里，即要无限地找0；遇到的1都装到q1里不动，即只找一“层”1，这样得到的1就是下一层的所有1
从1往外找0和1的时候，遇到的0都装到q0里不动，即只找一“层”0，这些0是作为下次拓展的种子；
遇到的1都放到q1里但不要动，即只找一“层”1，这就要求一开始有几个1要记录下来，这几个1拓展完了就不再拓展新来的1了，
然后这些新来的1还不是下一层的所有的1，还得用这一层的0再拓展出一些1，才能补全下一层的所有1
    





那么对于这个图，它的点和边的个数分别为：
* |V| = O(k * n^2)，k 是破墙次数，因为对于矩阵里的每个坐标点，都最多可能经过k次破墙达到它，那么对于整个图来说，所有的状态种类就一共有 k * n^2 种
* |E| = O(4 * k * n^2)，因为每个“状态”都可以向4个方向“走”，走到上下左右的另外4个状态，那么就相当于“边”的个数是“点”的个数的四倍

### Complexity
* Time: O(|V| + |E|)，因为worse情况可能需要全“图”遍历     <===== 对么？？
* Space: O(|V|)，因为BFS的queue里最多装|V|量级的状态点    <==== 对么？？

### Java
   《==== 下面这个实现对么 ？？？
```java
public class Solution {    
    private static final int[][] DIRS = {
        {0, 1}, {0, -1}, {1, 0}, {-1, 0}
    };
    
    public int minObstaclesToRemove(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        
        Queue<int[]> q0 = new LinkedList<>();
        Queue<int[]> q1 = new LinkedList<>();
        
        // 用一个长度为1的数组来传承当前累计破了几次墙，也就等于当前的所谓“layer”值
        int[] breakWallCount = new int[1];
                
        // [0, 0]处可能是0或1；[n-1, m-1]处也可能是0或1
        if (matrix[0][0] == 0) {
            q0.offer(new int[] {0, 0});
        } else { // == 1
            q1.offer(new int[] {0, 0});
        }
        
        // 已经访问过的cell都标-1，无论它原来是1还是0
        matrix[0][0] = -1;
        
        while (true) {
            if (bfsStartFromZeros(matrix, q0, q1, breakWallCount)) {
                return breakWallCount[0];
            }
            if (bfsStartFromOnes(matrix, q1, q0, breakWallCount)) {
                return breakWallCount[0];
            }
        }
    }
    
    private boolean bfsStartFromZeros(int[][] matrix, 
            Queue<int[]> q0, Queue<int[]> q1, int[] breakWallCount) {
        int n = matrix.length, m = matrix[0].length;
        // System.out.println("'0' Cells after break wall " + breakWallCount[0] + " times:");
        
        while (!q0.isEmpty()) {
            int[] curCell = q0.poll();
            int curX = curCell[0], curY = curCell[1];
            // System.out.println("    x: " + curX + ", y: " + curY);
            
            // 到达了整个矩阵的右下角
            if (curX == n - 1 && curY == m - 1) {
                return true;
            }
            
            for (int[] dir : DIRS) {
                int nextX = curX + dir[0], nextY = curY + dir[1];
                
                if (isValid(nextX, nextY, n, m)) {
                    if (matrix[nextX][nextY] == -1) {
                        continue;
                    } else if (matrix[nextX][nextY] == 0) {
                        q0.offer(new int[] {nextX, nextY});
                    } else { // == 1
                        q1.offer(new int[] {nextX, nextY});
                    }
                    matrix[nextX][nextY] = -1;
                }
            }
        }
        return false; // 终点不在这一层
    }
    
    private boolean bfsStartFromOnes(int[][] matrix, 
            Queue<int[]> q1, Queue<int[]> q0, int[] breakWallCount) {
        breakWallCount[0] += 1; // 处理1s意味着此时需要(再)破墙一次，即此时到了下一“层”
        // System.out.println("'1' Cells after break wall " + breakWallCount[0] + " times:");
       
        int n = matrix.length, m = matrix[0].length;
        
        int initialQ1Size = q1.size();
        int count = initialQ1Size;
        while (count > 0) {
            count--;
            int[] curCell = q1.poll();
            int curX = curCell[0], curY = curCell[1];
            // System.out.println("    x: " + curX + ", y: " + curY);
            
            // 到达了整个矩阵的右下角
            if (curX == n - 1 && curY == m - 1) {
                return true;
            }
            
            for (int[] dir : DIRS) {
                int nextX = curX + dir[0], nextY = curY + dir[1];
                
                if (isValid(nextX, nextY, n, m)) {
                    if (matrix[nextX][nextY] == -1) {
                        continue;
                    } else if (matrix[nextX][nextY] == 0) {
                        q0.offer(new int[] {nextX, nextY});
                    } else { // == 1
                        q1.offer(new int[] {nextX, nextY});
                    }
                    matrix[nextX][nextY] = -1;
                }
            }
        }
        return false; // 终点不在这一层
    }
    
    private boolean isValid(int x, int y, int n, int m) {
        return x >= 0 && x < n && y >= 0 && y < m;
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {

        int[][] matrix = {
            {1, 1, 1, 1, 0},
            {0, 1, 0, 1, 1},
            {0, 1, 0, 1, 1}
        };
        
        Solution solu = new Solution();
        
        // result is 4, means break wall 4 times
        System.out.println(solu.minObstaclesToRemove(matrix));
    }
}
```

## Reference
* 网上没有找到这题

{% include links.html %}
