---
title: "Add Elements in a Matrix, from {0, 0} to {x, y}"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_addInMatrix_00ToXY.html
folder: algorithm
toc: false
---

## Description：Google 电面题
给一个 int matrix，算它的 accumulation matrix，即左上角(0, 0)到右下角(i, j)的这个 sub matrix 里的所有数的和。举例：
```
输入
[[1, 2, 3],
[1, 3, 5],
[2, 4, 6]]
输出
[[1, 3, 6],
[2, 7, 15],
[4, 13, 27]]
```

### Example
见上文

## Solution 1：每一行从左到右算和，然后 result[i][j] = result[i - 1][j] + byRow[i][j - 1] + matrix[i][j]

### Complexity
* Time: O(n * m)
* Space: O(n * m)

### Java
```java
public class Solution {
    public int[][] accumulation(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return null;
        }
        
        int n = matrix.length;
        int m = matrix[0].length;
        int[][] byRows = new int[n][m];
        int[][] result = new int[n][m];
        
        // fill in the byRows grid
        for (int i = 0; i < n; i++) { // fill in the 1st col first
            byRows[i][0] = matrix[i][0];
        }
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < m; j++) {
                byRows[i][j] = byRows[i][j - 1] + matrix[i][j];
            }
        }
        
        // fill in the result grid
        result[0][0] = matrix[0][0]; // 左上角那一个点
        for (int j = 1; j < m; j++) { // 第一行
            result[0][j] = byRows[0][j];
        }
        for (int i = 1; i < n; i++) { // 第一列
            result[i][0] = result[i - 1][0] + matrix[i][0];
        }
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                result[i][j] = result[i - 1][j] + byRows[i][j - 1] + matrix[i][j];
            }
        }
        return result;
    }
}
```

## Solution 2：DP, dp[i][j] = dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - 1] + matrix[i][j]

### Complexity
* Time: O(n * m)
* Space: O(n * m)

### Java
```java
public class Solution {
    public int[][] accumulation(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return null;
        }
        
        int n = matrix.length;
        int m = matrix[0].length;
        int[][] result = new int[n][m];
        
        result[0][0] = matrix[0][0]; // 左上角那一个点
        for (int j = 1; j < m; j++) { // 第一行
            result[0][j] = result[0][j - 1] + matrix[0][j];
        }
        for (int i = 1; i < n; i++) { // 第一列
            result[i][0] = result[i - 1][0] + matrix[i][0];
        }
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                result[i][j] = 
                    result[i - 1][j] + result[i][j - 1] - result[i - 1][j - 1]
                    + matrix[i][j];
            }
        }
        return result;
    }

    public static void main(String[] args) {
        int[][] matrix = {{1,2,3}, {1,3,5}, {2,4,6}};
        Solution solu = new Solution();
        int[][] result = solu.accumulation(matrix);
        for (int[] array : result) {
            for (int ele : array) {
                System.out.print(ele + "  ");
            }
            System.out.println();
        }
    }
}
```

## Reference
网上没看到这题

{% include links.html %}
