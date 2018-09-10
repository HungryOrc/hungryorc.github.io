---
title: "Number of Distinct Islands"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_number_of_distinct_islands.html
folder: algorithm
toc: false
---

## Description
Given a non-empty 2D array `grid` of `0`s and `1`s, an island is a group of `1`s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of **distinct** islands. An island is considered to be the same as another if and only if one island can be translated (and NOT **rotated** or **reflected**) to equal the other.

Note: The length of each dimension in the given grid does not exceed 50.

### Example
* Input: 
  ```
  11011
  10000
  00001
  11011
  ```
  * Output: 3. Notice that:
    ```
    11   and    1    are considered to be different island shapes.
    1          11
    ```
    
## Solution
DFS无疑。但关键是如何对相同的形状去重。那么想到用HashSet，但在set里如何表达形状这个概念。容易想到左上角开始的展开，但何为左上角？可以设定一个矩阵从上到下，从左到右遍历，碰到的这个形状上的第一个点，就是这个形状的左上角。
对于一个特定的形状来说，这样的遍历方式所碰到的第一个点，一定是固定的，不会有第二种可能。而且这个点未必是这个形状的最左一列，但一定是最上一行。

然后，形状里的其他点的表征，最直觉的想法是它们相对于左上角点的相对坐标(x,y)，然后把这些(x,y)串成一个String。这个思路可以。

不过更高超的思路是用“**路径**”。只要是按照一定的顺序来进行一个点的向外拓展，比如这个顺序是“左右上下”，而且以后所有点都按“左右上下”的顺序来遍历，
那么这么遍历出来的路径过程一定是唯一的，做多少次都是一样的，更重要的是，有了这个路径，可以回推整个形状，回推的结果也是唯一的。

关键要注意：每一轮遍历的每一“层”，在走完这一层的时候，都要做一个特殊标记，以示“**回退**”的动作。这个特殊标记可以是任何char，比如空格。
回退可以是连续不止一次(即一层)的回退。比如（下面这些图里的s都表示starting point，s的位置上自然都必须是1，写s只是为了方便看）：
```
sRRDL     意味着    s11    过程是    s    s1    s11    s11    s11
                    11                                 1     11
```
```
sRR_DL    意味着    s11    过程是    s    s1    s11    s11    s11
                   11                                 1     11
```
```
sRR__DL   意味着    s11    过程是    s    s1    s11    s11    s11
                  11                                 1     11
```
```
sRR_D__L  意味着    s11    过程是    s    s1    s11    s11    1s11
                   11                                 1      1
```

### Complexity
* Time: O(n*m)，n是grid的行数，m是grid的列数
* Space: O(n*m)，最长的call stack的长度是把grid里的所有点都串在一个DFS路径里？？？ 这个答案对么 ？？？

### Java
我的code
```java
class Solution {
    private static final int[][] DIRS = new int[][]{{1,0}, {-1,0}, {0,1}, {0,-1}};
    private static final char[] DIR_CHARS = new int[]{'L', 'R', 'U', 'D'};
    
    public int numDistinctIslands(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        HashSet<String> islands = new HashSet<>();
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    grid[i][j] = 0;
                    
                    StringBuilder sb = new StringBuilder();
                    sb.append('s'); // 's' means the starting point
                    findDistinct(grid, i, j, sb);
                    
                    islands.add(sb.toString());
                }
            }
        }
        
        return islands.size();
    }
    
    private void findDistinct(int[][] grid, int x, int y, StringBuilder sb) {
        for (int i = 0; i < 4; i++) {
            int newX = x + DIRS[i][0];
            int newY = y + DIRS[i][1];
            if (isValidCoord(grid, newX, newY) && grid[newX][newY] == 1) {
                grid[newX][newY] = 0;
                
                char dirC = DIR_CHARS[i];
                sb.append(dirC);
                
                findDistinct(grid, newX, newY, sb);
            }
        }
        sb.append(' ');
    }
    
    private boolean isValidCoord(int[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length) {
            return false;
        }
        return true;
    }
}
```

## Reference
* [Number of Distinct Islands [LeetCode]](https://leetcode.com/problems/number-of-distinct-islands/description/)

{% include links.html %}
