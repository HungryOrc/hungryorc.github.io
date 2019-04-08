---
title: "Spiral Traversal of Rectangle Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_spiralTraversal_rectangleMatrix.html
folder: algorithm
toc: false
---

## Description
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

### Example
* Input: 
  ```
  [ 1, 2, 3 ],
  [ 4, 5, 6 ],
  [ 7, 8, 9 ]
  ```
  * Output: [1,2,3,6,9,8,7,4,5]
* Input: 
  ```
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
  ```
  * Output: [1,2,3,4,8,12,11,10,9,5,6,7]

## Solution：我自己的方法，速度前1%
每个矩阵都剥洋葱，一层一层，关键是每一剥都不要剥到底，剥到还差一个元素就停，那么四个方向的剥洋葱就是这个样子：
```
1 1 1 1 1 2
4         2
4 3 3 3 3 3
```
第二，要特别注意！如果剥到中心只剩下一行或者一列了，或者一开始就只有一行或者一列，那么要特殊处理，就直接遍历这一行/列 就行了！

### Complexity
* Time: O(n * m)
* Space: O(1)

### Java
代码虽然看起来不短，其实逻辑挺简明的
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return result;
        }
        
        int n = matrix.length; // number of rows
        int m = matrix[0].length; // number of cols
        int topRow = 0, bottomRow = n - 1;
        int leftCol = 0, rightCol = m - 1;
        
        while (topRow <= bottomRow && leftCol <= rightCol) {
            int width = rightCol - leftCol;
            int height = bottomRow - topRow;
            
            if (height == 0) { // 只剩最后一行了
                for (int i = leftCol; i <= rightCol; i++) {
                    result.add(matrix[topRow][i]);
                }
                break;
            }
            
            if (width == 0) { // 只剩最后一列了
                for (int i = topRow; i <= bottomRow; i++) {
                    result.add(matrix[i][leftCol]);
                }
                break;
            }
            
            for (int i = 0; i < width; i++) {
                result.add(matrix[topRow][leftCol + i]);
            }
            for (int i = 0; i < height; i++) {
                result.add(matrix[topRow + i][rightCol]);
            }
            for (int i = 0; i < width; i++) {
                result.add(matrix[bottomRow][rightCol - i]);
            }
            for (int i = 0; i < height; i++) {
                result.add(matrix[bottomRow - i][leftCol]);
            }
            
            topRow++;
            bottomRow--;
            leftCol++;
            rightCol--;
        }
        return result;
    }
}
```

## Reference
* [Spiral Matrix [LeetCode]](https://leetcode.com/problems/spiral-matrix/description/)

{% include links.html %}
