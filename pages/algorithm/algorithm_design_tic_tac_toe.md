---
title: "Design Tic-Tac-Toe"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_design_tic_tac_toe.html
folder: algorithm
toc: false
---

## Description
Design a Tic-tac-toe game that is played between two players on a n x n grid.

You may assume the following rules:
1. A move is guaranteed to be valid and is placed on an empty block.
2. Once a winning condition is reached, no more moves is allowed.
3. A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

我们默认每次每个玩家落子的地方都是valid的，即我们不必操心玩家怎么落子，他想落在哪里，他选择落子位置的时间复杂度算法等等。

Your TicTacToe object will be instantiated and called as such:
```java
TicTacToe obj = new TicTacToe(n);
int param_1 = obj.move(row,col,player);
```

Follow up: Could you do better than O(n^2) per move() operation?

### Example
```
Given n = 3, assume that player 1 is "X" and player 2 is "O" in the board.

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -> Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -> Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
```

## Solution 1: 很巧妙的方法，可能有点过于机巧了
Ref: https://leetcode.com/problems/design-tic-tac-toe/

The key observation is that in order to win Tic-Tac-Toe you must have the entire row or column. 
Thus, we don't need to keep track of an entire n^2 board. We only need to keep a count for each row and column. 

If at any time a row or column matches the size of the board then that player has won.
To keep track of which player, I add one for Player1 and -1 for Player2. 

There are two additional variables to keep track of the count of the diagonals. 8
Each time a player places a piece we just need to check the count of that row, column, diagonal and anti-diagonal.

注意！如果这题是一个 OOD 的设计题，那么要注意以下的 和玩家落子即move函数相关的 特殊情况：
* 最后可能无子可落，即 **棋盘已经落满** 的情况
* 还没落满，但 **每个行列和两个对角线都已经没有任何一方赢的可能性了**。即 每一行，每一列，以及2个对角线，都已经有了两个玩家的落子

### Complexity
* Time: O(1)，这是 任何一个玩家落子一次以及判断输赢 的时间复杂度。即move函数run一次的TC
* Space: O(n)，n是矩阵的行数和列数里更大的那一个

### Java
```java
public class TicTacToe 
{
    private int size;
    private int[] rows; // sum of elements in each row
    private int[] cols; // sum of elements in each column
    private int diagonal, antiDiagonal;

    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        size = n;
        rows = new int[n];
        cols = new int[n];
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) 
    {
        int toAdd = 1;
        if (player == 2)
            toAdd = -1;
        
        // manage rows and columns
        rows[row] += toAdd;
        if (Math.abs(rows[row]) == size) {
            return player;
        }
        
        cols[col] += toAdd;
        if (Math.abs(cols[col]) == size) {
            return player;
        }
        
        // manage diagonal and antiDiagonal
        if (row == col) {
            diagonal += toAdd;
            if (Math.abs(diagonal) == size) {
                return player;
            }
        }
        
        if (row + col == size - 1) {
            antiDiagonal += toAdd;
            if (Math.abs(antiDiagonal) == size) {
                return player;
            }
        }
        
        return 0; // no one won
    }
}
```

## Reference
* [Design Tic-Tac-Toe [LeetCode]](https://leetcode.com/problems/design-tic-tac-toe/description/)

{% include links.html %}
