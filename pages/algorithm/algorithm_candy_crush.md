---
title: "Candy Crush"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_candy_crush.html
folder: algorithm
toc: false
---

## Description
This question is about implementing a basic elimination algorithm for Candy Crush.

Given a 2D integer array board representing the grid of candy, different positive integers board[i][j] represent different types of candies. A value of board[i][j] = 0 represents that the cell at position (i, j) is empty. The given board represents the state of the game following the player's move. Now, you need to restore the board to a stable state by crushing candies according to the following rules:
* If three or more candies of the same type are adjacent vertically or horizontally, "crush" them all at the same time - these positions become empty.
* After crushing all candies simultaneously, if an empty space on the board has candies on top of itself, then these candies will drop until they hit a candy or bottom at the same time. (No new candies will drop outside the top boundary.)
* After the above steps, there may exist more candies that can be crushed. If so, you need to repeat the above steps.
* If there does not exist more candies that can be crushed (ie. the board is stable), then return the current board.
You need to perform the above rules until the board becomes stable, then return the current board.

Note:
* The length of board will be in the range [3, 50].
* The length of board[i] will be in the range [3, 50].
* Each board[i][j] will initially start as an integer in the range [1, 2000].

### Example
原题有个很精美的大图，详述了流程：https://leetcode.com/problems/candy-crush/description/

## Solution：我自己的方法，每个cell只考察右方和下方！速度还行
**每一轮先标记所有要crush的cells，扫完一遍矩阵了再crush，最后再逐列处理下落，这样算是完成了一轮。**

### Complexity
* Time: O(n^2 * m^2) <==== ？？？？
* Space: O(nm)

### Java
看起来长，其实不复杂。helper function有不少而已
```java
public class Solution {
    public int[][] candyCrush(int[][] board) {
        // ignore sanity checks
        
        int n = board.length, m = board[0].length;
        boolean[][] cellsToCrush = new boolean[n][m];
        boolean crushCells = false;
        
        do {
            // 我们只往右边和下面看，而且题目要求成三才能消除
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    findByRowDownwardsAndByColRightwards(board, i, j, cellsToCrush);
                }
            }

            crushCells = false;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    if (cellsToCrush[i][j]) {
                        board[i][j] = 0;
                        cellsToCrush[i][j] = false;
                        crushCells = true;
                    }
                }
            }            

            for (int j = 0; j < m; j++) {
                removeZeroByCol(board, j);
            }
        } while (crushCells);
        
        return board;
    }
    
    private void findByRowDownwardsAndByColRightwards(
            int[][] board, int i, int j, boolean[][] cellsToCrush) {
        
        // 特别注意！这里不能用 cellsToCrush[i][j] 作为提前return的标准！这个地方很容易犯错！
        //
        // 之所以不行，是因为 一个cell被注定要crush以后，比如它是被同row的其他cell带动crush的，
        // 但它本身可能还能trigger它所在的col上的其他cells也一起被crush！所以现在就把它
        // 剔除出考察名单，是不对的！
        // 但是真的crush以后，即真的成了0以后，自然是不必再考察它了
        if (board[i][j] == 0) return;
        
        int originalValue = board[i][j];
        
        // 如果当前cell下方紧贴的cell与当前cell的value相同
        if (i + 2 < board.length && board[i + 1][j] == originalValue) {
            int end = i + 1;
            for (; end < board.length; end++) {
                if (board[end][j] != originalValue) break;
            }
            end--; // 别忘了这个
            if (end - i + 1 >= 3) {
                for (int index = i; index <= end; index++) 
                    cellsToCrush[index][j] = true;
            }
        }
        
        if (j + 2 < board[0].length && board[i][j + 1] == originalValue) {
            int end = j + 1;
            for (; end < board[0].length; end++) {
                if (board[i][end] != originalValue) break;
            }
            end--; // 别忘了这个
            if (end - j + 1 >= 3) {
                for (int index = j; index <= end; index++) 
                    cellsToCrush[i][index] = true;
            }
        }
    }
    
    private void removeZeroByCol(int[][] matrix, int col) {
        int slow = matrix.length - 1, fast = matrix.length - 1;
        
        while (fast >= 0) {
            while (slow >= 0 && matrix[slow][col] != 0) slow--;
            if (slow < 0) return; 
            
            fast = slow - 1;
            while (fast >= 0 && matrix[fast][col] == 0) fast--;
            if (fast < 0) return;
            
            swap(matrix, col, slow, fast);
            slow--;
            fast--;
        }
    }
    
    private void swap(int[][] matrix, int col, int i, int j) {
        int tmp = matrix[i][col];
        matrix[i][col] = matrix[j][col];
        matrix[j][col] = tmp;
    }
}
```

## Reference
* [Candy Crush [LeetCode]](https://leetcode.com/problems/candy-crush/description/)

{% include links.html %}
