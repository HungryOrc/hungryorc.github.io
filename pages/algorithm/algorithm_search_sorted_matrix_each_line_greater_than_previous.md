---
title: "Search in Sorted Matrix, each line's first element is greater than previous line's last element"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_search_sorted_matrix_each_line_greater_than_previous.html
folder: algorithm
toc: false
---

## Description
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

### Example
* Input: matrix = [[1,   3,  5,  7], [10, 11, 16, 20], [23, 30, 34, 50]], target = 3
  * Output: true

## Solution 1: 先找行，再在那一行里找target

### Complexity
* Time: O(logn + logm) = O(log(n * m))
* Space: O(1)

### Java
用的是
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        // 找一行的第一个元素比target小，但又是最大的一行
        int sRow = 0, eRow = matrix.length - 1; // start row and end row
        while (sRow + 1 < eRow) { 
            int mRow = sRow + (eRow - sRow) / 2; // middle row
            if (matrix[mRow][0] > target) {
                eRow = mRow;
            } else if (matrix[mRow][0] < target) {
                sRow = mRow;
            } else {
                return true;
            }
        }
        
        // 此时 sRow + 1 = eRow，我们要找最大的行，所以先查 eRow
        int targetRow = -1;
        if (matrix[eRow][0] <= target) {
            targetRow = eRow;
        } else if (matrix[sRow][0] <= target) {
            targetRow = sRow;
        } else {
            return false; // 到了这里意味着 最小的一行的第一个元素也比target大
        }
        
        int sCol = 0, eCol = matrix[0].length - 1; // start col and end col
        while (sCol + 1 < eCol) {
            int mCol = sCol + (eCol - sCol) / 2; // middle col
            if (matrix[targetRow][mCol] > target) {
                eCol = mCol;
            } else if (matrix[targetRow][mCol] < target) {
                sCol = mCol;
            } else {
                return true;
            }
        }
        
        return (matrix[targetRow][sCol] == target ||
                matrix[targetRow][eCol] == target);
    }
}
```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
