---
title: "Knight Probability in Chessboard"
tags: [algorithm, DP]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_knight_probability_chessboard.html
folder: algorithm
toc: false
---

## Description
On an NxN chessboard, a knight starts at the r-th row and c-th column and attempts to make exactly K moves. The rows and columns are 0 indexed, so the top-left square is (0, 0), and the bottom-right square is (N-1, N-1).

A chess knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly K moves or has moved off the chessboard. Return the probability that the knight remains on the board after it has stopped moving.

Note:
* N will be between 1 and 25.
* K will be between 0 and 100.
* The knight always initially starts on the board.

### Example
* Input: 3, 2, 0, 0
  * Output: 0.0625
  * There are two moves (to (1,2), (2,1)) that will keep the knight on the board. From each of those positions, there are also two moves that will keep the knight on the board.
The total probability the knight stays on the board is 0.0625.
    
## Solution
Ref: https://leetcode.com/problems/knight-probability-in-chessboard/discuss/108181/My-accepted-DP-solution

这一题我用 iteration (by queue) + 记忆化存储 做了很久，都没能达到memory的要求，因为我做的记忆化只是一步的记忆化，没有做到底。要把记忆化做到底只能 DP。

* 对于棋盘上的任一点，从它出发，走0步，不出棋盘，这个走法自然是1种，也就是不走
* 然后，对于棋盘上的任一点，从它出发，走1步，不出棋盘，总的走法的个数，等于它一步能走到多少个棋盘内的点，也就是把前一步的那些1加起来。
如果一个点往外走的8种情况里，3种出了棋盘，5种没出，对于这5个目标点，从它们出发再走0步不出棋盘共有1种走法，所以5个1加在一起等于5，就是前一个点走一步
不出棋盘一共有5种走法
* 对于棋盘上的任一点，从它出发，走2步，不出棋盘，总的走法的个数，等于它一步能走到多少个棋盘内的点，然后从那些点出发走一步不出棋盘各自有多少种走法，
那些走法都加在一起，就是从一开始那个点走2步不出棋盘的走法
* 以此类推......

注意！这个方法里，用第n+1步反推第n步的时候，在推完所有的第n步之前，不能改变第n+1步的DP矩阵！不然就错了！比如，每一个点走0步，DP矩阵上每一点都是1，
但我们在求每个点走1步的时候，必须另开一个新矩阵来做走一步的情况，不能动前面那个矩阵里的那些1，等一步的情况做完了，再把老的DP矩阵(都是1)用新矩阵来代替。

### Complexity
* Time: `O(n*m)`，n是grid的行数，m是grid的列数
* Space: `O(n*m)`，最长的call stack的长度是把grid里的所有点都串在一个DFS路径里，那么这次DFS就有 n*m 层，每层空间消耗 O(1)

### Java
```java
public class Solution {
    
    private static final int[][] DIRS = {
        {2,1},{2,-1},{-2,1},{-2,-1},{1,2},{1,-2},{-1,2},{-1,-2}
    };
    
    public double knightProbability(int N, int K, int r, int c) {
        // sanity checks
        if (N <= 0 || K < 0) {
            return -1.0;
        }
        if (!isOnBoard(N, r, c)) {
            return 0.0;
        }
        if (K == 0) {
            return 1.0;
        }
        
        double[][] nextStepDP = new double[N][N];
        // initialize all the elements in the dp grid to be 1
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                nextStepDP[i][j] = 1;
            }
        }
        
        for (int round = 1; round <= K; round++) {
            double[][] curStepDP = new double[N][N];
            
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    
                    for (int d = 0; d < DIRS.length; d++) {
                        int nextR = i + DIRS[d][0];
                        int nextC = j + DIRS[d][1];
                        if (isOnBoard(N, nextR, nextC)) {
                            curStepDP[i][j] += nextStepDP[nextR][nextC];
                        }
                    }
                }
            }
            nextStepDP = curStepDP;
        }
        
        return nextStepDP[r][c] / Math.pow(8, K);
    }
    
    private boolean isOnBoard(int N, int row, int col) {
        if (row < 0 || row >= N || col < 0 || col >= N) {
            return false;
        }
        return true;
    }
}
```

## Reference
* [Knight Probability in Chessboard [LeetCode]](https://leetcode.com/problems/knight-probability-in-chessboard/description/)

{% include links.html %}
