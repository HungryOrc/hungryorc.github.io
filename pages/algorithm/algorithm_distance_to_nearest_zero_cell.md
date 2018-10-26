---
title: "Distance to Nearest Zero Cell"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_distance_to_nearest_zero_cell.html
folder: algorithm
toc: false
---

## Description
Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell. The distance between two adjacent cells is 1.
* The number of elements of the given matrix will not exceed 10,000.
* There are at least one 0 in the given matrix.
* The cells are adjacent in only four directions: up, down, left and right. 斜的不行

这题就是说，对于每个非0的cell，确定它到最近的为0的cell的距离是多少。

### Example
* Input:
  ```
  0 0 0 
  0 1 0
  1 1 1
  ```
  * Output:
    ```
    0 0 0 
    0 1 0
    1 2 1
    ```

## Solution
用DP做：一个非0的cell到最近的为0的cell的最短距离，等于它四周的cell（是不是0都要算，因为0 cell到0的距离为0）到离它们最近的0 cell的距离（这样就有4个距离值）里面取最小的一个，然后 +1

注意：
* 最关键之处在于：要分两个方向做！先左上到右下做一次，再右下到左上反过来做一次！
  * 当然也可以从上往下，从下往上，从左往右，从右往左做四次。这样的结果是一样的。这种做法可能更符合直觉。上面讲的斜着来回做两次的方法，其实是上下左右做四次的方法的优化结果
* 第二关键之处在于：在从左上到右下的过程中，最左边的一列没有左邻，最上的一行没有上邻，左上角的那个cell没有左或上邻。从右下往左上的过程中，同理类似

### Complexity
* Time: O(n * m)，遍历原矩阵几次即可
* Space: O(n * m), DP矩阵

### Java
```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return null;
        }
        
        int n = matrix.length;
		int m = matrix[0].length;
		int[][] result = new int[n][m];

		// 初始填空
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (matrix[i][j] == 0) {
					result[i][j] = 0;
                } else {
	                result[i][j] = Integer.MAX_VALUE;
                }
            }
        }

        // 从左上到右下的填空
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 0) {
                    continue;
                }
                if (i > 0 && result[i - 1][j] < Integer.MAX_VALUE) {
                    result[i][j] = Math.min(result[i - 1][j] + 1, result[i][j]);
                }
                if (j > 0 && result[i][j - 1] < Integer.MAX_VALUE) {
                    result[i][j] = Math.min(result[i][j - 1] + 1, result[i][j]);
                }
            }
        }

        // 从右下到左上的填空
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                if (matrix[i][j] == 0) {
                    continue;
                }
                if (i < n - 1 && result[i + 1][j] < Integer.MAX_VALUE) {
                    result[i][j] = Math.min(result[i + 1][j] + 1, result[i][j]);
                }
                if (j < m - 1 && result[i][j + 1] < Integer.MAX_VALUE) {
                    result[i][j] = Math.min(result[i][j + 1] + 1, result[i][j]);
                }
            }
        }
        
        return result;
    }
}
```

## Reference
* [01 Matrix [LeetCode]](https://leetcode.com/problems/01-matrix/description/)

{% include links.html %}
