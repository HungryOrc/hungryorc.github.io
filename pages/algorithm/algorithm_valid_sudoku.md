---
title: "Valid Sudoku"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_valid_sudoku.html
folder: algorithm
toc: false
---

## Description
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

The Sudoku board could be partially filled, where empty cells are filled with the character `.`. 这里有配图，见[此处](https://leetcode.com/problems/valid-sudoku/description/)

### Example
此处是一些图，见原题出处：https://leetcode.com/problems/valid-sudoku/description/

## Solution：一种比较巧妙的做法，不用多个set，只用一个set搞定。弊端是，每次要构建strings再放到set里去，做string很慢

### Complexity
* Time: O(nm)
* Space: O(nm) <==== ？？？？ set可能的最大size是这么大么？

### Java
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return true;
        }
        
        int n = board.length, m = board[0].length;
        Set<String> set = new HashSet<>();
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                char curChar = board[i][j];
                if (curChar == '.') continue; // 别忘了这个特殊处理
                
                if (!set.add("row" + i + "-" + curChar)) return false;
                if (!set.add("col" + j + "-" + curChar)) return false;
                
                String curSubBox = "box" + (i / 3) + (j / 3);
                if (!set.add(curSubBox + "-" + curChar)) return false;
            }
        }
        return true;
    }
}
```

## Reference
* [Valid Sudoku [LeetCode]](https://leetcode.com/problems/valid-sudoku/description/)

{% include links.html %}
