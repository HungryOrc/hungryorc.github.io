---
title: "Shortest Path in a Matrix with Wall Breaking"
tags: [algorithm]
keywords: wall, obstacle
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortest_path_in_matrix_with_wall_breaking.html
folder: algorithm
toc: false
---

## Description
一个正方形迷宫，边长n，里面1的cell是墙，0的cell是路。每次可以走上下左右四个方向。给定起始点s和终止点t。可以破墙，最多k次。问从s到t的最短路径是多长。一个被破的墙的cell也算距离1，
一个原本就是路点的cell也算距离1。

### Example
* Input:
  * Output:

## Solution
用图论来做这题。所谓的“图”里的每个“点”是这么一个状态：(x, y, b)，意思是破墙b次后到达矩阵里坐标 (x, y) 处。
* 从起始点向外逐层扩散，BFS
* 没遇到墙的地方，和普通的迷宫走路题一样
* 遇到墙的地方，可以破，也可以不破，两种选择都ok，形成两个支路。当然如果k次破墙法术用尽了，那就不能再破了
* 终止条件：首次走到终点坐标，而且破墙次数 <= k。即达到状态 (xt, yt, bt)，其中 bt <= k

那么对于这个图，它的点和边的个数分别为：
* |V| = O(k * n^2)，k 是破墙次数，因为对于矩阵里的每个坐标点，都最多可能经过k次破墙达到它，那么对于整个图来说，所有的状态种类就一共有 k * n^2 种
* |E| = O(4 * k * n^2)，因为每个“状态”都可以向4个方向“走”，走到上下左右的另外4个状态，那么就相当于“边”的个数是“点”的个数的四倍

### Complexity
* Time: O(|V| + |E|)，因为worse情况可能需要全“图”遍历     <===== 对么？？
* Space: O(|V|)，因为BFS的queue里最多装|V|量级的状态点    <==== 对么？？

### Java
下面这个代码看起来不断，其实逻辑很简明   《==== 下面这个实现对么 ？？？
```java
public class Solution {
    private final static int[][] DIRS = {
            {0, 1}, {0, -1}, {-1, 0}, {1, 0}
    };
    
    // sx, sy is the coordinates of the starting point
    // tx, ty is the coordinates of the terminal point
    // k is the max number of wall breakings allowed
    public int matrixBreakWalls(int[][] matrix, int sx, int sy, int tx, int ty, int k) {
        // ignore sanity checks
        
        int len = matrix.length; // matrix is an n*n square
        
        // 每个“状态点”用一个长度为3的int数组来表示
        // 第一个元素表示当前x，第二个元素表示当前y，第三个元素表示当前已经破了几次墙
        Integer[] initState = {sx, sy, 0};
        
        // 我们要记录每个“状态是否已经达到过”，对于BFS，已经达到过的状态就不再以它为中心展开了
        // 这里特别注意！对于同一个坐标点(x, y)，破墙1次到达它，和破墙3次到达它，和不破墙到达它，
        // 是不同的状态！
        boolean[][][] visited = new boolean[len][len][k]; 
        
        Queue<Integer[]> states = new LinkedList<>();
        states.offer(initState);
        visited[sx][sy][0] = true;
        int level = 0;
        
        while (!states.isEmpty()) {
            int curLevelSize = states.size();
            level ++;
            
            for (int i = 0; i < curLevelSize; i++) {    
                Integer[] curState = states.poll();
                int curX = curState[0];
                int curY = curState[1];
                int curBk = curState[2];
                
                // 符合要求到达终点
                if (curX == tx && curY == ty && curBk <= k) {
                    return level;
                }
                
                for (int j = 0; j < 4; j++) {
                    int newX = curX + DIRS[i][0];
                    int newY = curY + DIRS[i][1];
                    
                    if (matrix[newX][newY] == 0) { // 下一步是路
                        if (visited[newX][newY][curBk] == false) {
                            visited[newX][newY][curBk] = true;
                            states.offer(new Integer[]{newX, newY, curBk});
                        }
                    } else { // 下一步是墙
                        // 那么要走进这个cell就必须破墙
                        // 那么第一要看，目前破墙次数用光没，
                        // 第二要看，在目前的基础上再破一次墙进这块墙里，这个状态我们访问过没
                        if (curBk + 1 < k) {
                            if (visited[newX][newY][curBk + 1] == false) {
                                visited[newX][newY][curBk + 1] = true;
                                states.offer(new Integer[] {newX, newY, curBk + 1});
                            }
                        }
                    }
                }
            }
        }
        
        // 有可能我们最后耗尽了k也达不到终点，那么就返回正无穷（步数）
        return Integer.MAX_VALUE;
    }
}
```

## Reference
* 网上没有找到这题

{% include links.html %}
