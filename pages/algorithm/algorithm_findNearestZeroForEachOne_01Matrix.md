---
title: "01 Matrix: Find the Distance to the Nearest Zero for all Ones in the Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_findNearestZeroForEachOne_01Matrix.html
folder: algorithm
toc: false
---

## Description
Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell. The distance between two adjacent cells is 1.

Note:
* The number of elements of the given matrix will not exceed 10,000.
* There are at least one 0 in the given matrix.
* The cells are adjacent in only four directions: up, down, left and right.

Clearification Question: 是否能改变输入的matrix？

Follow-up Quesion：如果要求 in place 做，那怎么做？

### Example
* Input: 
  ```
  0,0,0
  0,1,0
  1,1,1
    ```
  * Output: 
    ```
    0,0,0
    0,1,0
    1,2,1    
    ```

## Solution：KBFS，且 in place

### Complexity
* Time: O(n * m)
* Space: O(1), in place! 要做到这一点，就不能使用visited矩阵。那么要注意对原int矩阵的特殊处理方式

### Java
```java
class Solution {
	private static final int[][] DIRS = {
        {0, 1}, {0, -1}, {1, 0}, {-1, 0}
    };

    public int[][] updateMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return null;

        int n = matrix.length, m = matrix[0].length;

        // get all zero cells
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 0) {
                    queue.add(new int[]{i, j});
                } else {
                    matrix[i][j] = -1; // mark as un-touched
                }
            }
        }	

        int label = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                int[] curCoord = queue.poll();
                int x = curCoord[0], y = curCoord[1];

                for (int[] dir : DIRS) {
                    int newX = x + dir[0];
                    int newY = y + dir[1];

                    // out of bound 必须放在最前面！！！
                    if (outOfBound(newX, newY, n, m) || matrix[newX][newY] != -1) {
                        continue;
                    }

                    matrix[newX][newY] = label + 1;
                    queue.offer(new int[]{newX, newY});
                }
            }
            label++;
        }
        return matrix;
    }

    private boolean outOfBound(int x, int y, int n, int m) {
        return x < 0 || y < 0 || x == n || y == m;
    }
}
```

## Reference
* [01 Matrix [LeetCode]](https://leetcode.com/problems/01-matrix/description/)

{% include links.html %}
