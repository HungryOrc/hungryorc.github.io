---
title: "Toeplitz Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_toeplitz_matrix.html
folder: algorithm
toc: false
---

## Description
A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same element.

Now given an M*N matrix, return True if and only if the matrix is Toeplitz.
* matrix will be a 2D array of integers.
* matrix will have a number of rows and columns in range [1, 20].
* matrix[i][j] will be integers in range [0, 99].

### Follow Ups
1. What if the matrix is stored on disk, and the memory is limited such that you can only load at most one row of the matrix into the memory at once?
2. What if the matrix is so large that you can only load up a partial row into the memory at once?
3. What if there are too many rows and/or columns that it would be very slow for one machine to deal with all the data?

### Example
* Input: 
  ```
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
  ```
  * Output: True
  * Explanation: In the above grid, the diagonals are: "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]". In each diagonal all elements are the same, so the answer is True.


## Solution
哦也

### Complexity
* Time: O(n*m)
* Space: O(n)，map占用n空间

### Java
```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        // <cell value on this diagonal, diff of x-y on this diagonal>
        Map<Integer, Integer> map = new HashMap<>();
        int n = matrix.length, m = matrix[0].length;
        
        // 把第一列的各个元素所属的对角线的情况装入map
        for (int diagDiff = 0; diagDiff < n; diagDiff++) {
            map.put(diagDiff, matrix[diagDiff][0]);
        }
        // 把第一行的各个元素所述的对角线的情况装入map
        for (int diagDiff = -1; diagDiff > -m; diagDiff--) {
            // 注意，这里用diagDiff做matrix里的index的时候，要乘以 -1
            map.put(diagDiff, matrix[0][-1 * diagDiff]);
        }
        
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                if (matrix[i][j] != map.get(i - j)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

## Reference
* [Toeplitz Matrix [LeetCode]](https://leetcode.com/problems/toeplitz-matrix/description/)

{% include links.html %}
