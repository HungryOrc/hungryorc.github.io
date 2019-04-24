---
title: "Shortest Distance in Matrix: Between any A and B, Manhattan Distance"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortestDistanceInMatrix_anyAAndB_manhattan.html
folder: algorithm
toc: false
---

## Description
在一个矩阵里，2个cell之间的manhattan distance 是 `|x1 - x2| + |y1 - y2|`。那么对于一个cell A 来说，它周围的manhattan distance 为 1，2，3 的cells分别为
`*`，`+`，`#`所标示的那些cells：
```
      #
    # + #
  # + * + #
# + * A * + #
  # + * + #
    # + #
      #
```
所以如果要用程序的方式来表达距离一个点的manhattan 距离为n 的cells 的 坐标s，则有点费时间。需要把个4斜向分段的坐标都写出来。
换一个思路就好做多了：对于BFS里的每一层的cells，只展开它上下左右四个正对方向的cells，即只展开manhattan distance为1的那些店。重复的则不管它

### Example
略

## Solution：BFS

### Complexity
* Time: O(n * m)，不可能更低了
* Space: O(n * m), queue size

### Java
逻辑其实很简明。看起来代码不短
```java
public class Solution {
    private static final int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}
    };
    
    public int shortestDist2D(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return -1;
        }
        
        int n = matrix.length;
        int m = matrix[0].length;
        List<Integer[]> coordsA = new ArrayList<>();
        List<Integer[]> coordsB = new ArrayList<>();
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 'A') {
                    Integer[] coords = {i, j};
                    coordsA.add(coords);
                } else if (matrix[i][j] == 'B') {
                    Integer[] coords = {i, j};
                    coordsB.add(coords);
                }
            }
        }
        
        if (coordsA.size() == 0 || coordsB.size() == 0) {
            return -1;
        }
        
        boolean[][] visited = new boolean[n][m];
        Queue<Integer[]> queue = new LinkedList<>();
        // put all the coords of A into the queue, and try to find a nearest B
        // we can also do the opposite: put Bs into queue and find A, 时间复杂度是一样的！不管A多还是B多！
        for (Integer[] coord : coordsA) {
            queue.offer(coord);
        }
        int dist = 0;
        
        while (!queue.isEmpty()) {
            dist++;
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                Integer[] curCoord = queue.poll();
                int curX = curCoord[0];
                int curY = curCoord[1];
                
                for (int[] dir : DIRS) {
                    int x = curX + dir[0];
                    int y = curY + dir[1];
                    
                    if (!inBound(x, y, n, m)) {
                        continue;
                    }
                    
                    if (visited[x][y]) {
                        continue;
                    }
                    
                    if (matrix[x][y] == 'B') {
                        return dist;
                    }
                    
                    visited[x][y] = true;
                    queue.offer(new Integer[]{x, y});
                }
            }
        }
        return -1; // 其实永远不会到这里，因为如果没有A或B，早就在之前就返回-1了
    }
    
    private boolean inBound(int x, int y, int n, int m) {
        return x >= 0 && x < n && y >= 0 && y < m;
    }

    public static void main(String[] args) {
        char[][] matrix = {{'A', 'O', 'X'},
                           {'X', 'L', 'B'},
                           {'N', 'A', 'C'}};
        
        Solution solu = new Solution();
        int result = solu.shortestDist2D(matrix);
        System.out.println(result); // 2
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
