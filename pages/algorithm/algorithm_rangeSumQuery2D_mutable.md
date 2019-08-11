---
title: "Range Sum Query 2D - Mutable"
tags: [algorithm, segment_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rangeSumQuery2D_mutable.html
folder: algorithm
toc: false
---

## Description: Hard，Google的题
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).
配图见[此](https://leetcode.com/problems/range-sum-query-2d-mutable/description/).

```java
// Your NumArray object will be instantiated and called as such:
NumMatrix obj = new NumMatrix(matrix);
obj.update(row,col,val);
int param_2 = obj.sumRegion(row1,col1,row2,col2);
```

Note:
* The array is only modifiable by the update function.
* You may assume the number of calls to update and sumRange function is distributed evenly.

### Example
* Input:
  ```
  matrix = [
      [3, 0, 1, 4, 2],
      [5, 6, 3, 2, 1],
      [1, 2, 0, 1, 5],
      [4, 1, 0, 1, 7],
      [1, 0, 3, 0, 5]
  ]
  ```
  * Output: 
    ```
    ssumRegion(2, 1, 4, 3) -> 8
    update(3, 2, 2)
    sumRegion(2, 1, 4, 3) -> 10
    ```

## Solution 1: 我的方法：上下左右四分法的Segment Tree！我还从没见任何人这么做过。速度 前70%，有点慢。但是逼格爆表！
可以对照着 Range Sum Query 1D (Array) - Mutable 那题的经典的 二分的Segment Tree 的做法来看，就容易理解这个 空前绝后的 四分Segment Tree 了。

### Complexity
* Time: O((nm)log(nm)) <==== ？？？？
* Space: O(nm) <==== ？？？？

### Java
代码长。但思路不是很复杂。
```java
class SegTreeNode {
    int x1, y1; // (x1, y1) is top left corner
    int x2, y2; // (x2, y2) is bottom right corner
    
    int sum;
    
    SegTreeNode nTL; // child node representing the top left part of the region of cur node
    SegTreeNode nTR; // top right
    SegTreeNode nBL; // bottom left
    SegTreeNode nBR; // bottom right
    
    public SegTreeNode(int x1, int y1, int x2, int y2, int sum) {
        this.x1 = x1;
        this.y1 = y1;
        this.x2 = x2;
        this.y2 = y2;
        this.sum = sum;
    }
}

class SegTree {
    int[][] matrix;
    SegTreeNode root;
    
    public SegTree(int[][] matrix) {
        this.matrix = matrix;
        
        int n = matrix.length;
        int m = matrix[0].length;

        this.root = new SegTreeNode(0, 0, n - 1, m - 1, 0);
        initialPopulateTree(this.root, this.matrix);
    }
    
    private void initialPopulateTree(SegTreeNode root, int[][] matrix) {
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                add(root, i, j, matrix[i][j]);
            }
        }
    }
    
    public void add(SegTreeNode node, int x, int y, int val) {
        node.sum += val;
        
        int x1 = node.x1, y1 = node.y1;
        int x2 = node.x2, y2 = node.y2;
        
        if (x1 == x2 && y1 == y2) { // 当前node 所代表的range 缩小到了一个点
            return;
        }
        
        // left到mid算 左子树，mid+1到right算 右子树。水平和竖直方向都遵循这个做法
        int midX = x1 + (x2 - x1) / 2;
        int midY = y1 + (y2 - y1) / 2;
        
        // 这里就要分4种情况了，因为这是矩阵！不像数组的 sum query 那样只用分2种情况
        // 情况1：新加入的cell的坐标 在左上分区
        if (x <= midX && y <= midY) {
            if (node.nTL == null) {
                SegTreeNode childTL = new SegTreeNode(x1, y1, midX, midY, 0);
                node.nTL = childTL;
            }
            add(node.nTL, x, y, val);
        }
        // 情况2：新加入的cell的坐标 在右上分区
        else if (x <= midX && y > midY) {
            if (node.nTR == null) {
                SegTreeNode childTR = new SegTreeNode(x1, midY + 1, midX, y2, 0);
                node.nTR = childTR;
            }
            add(node.nTR, x, y, val);
        }
        // 情况3：新加入的cell的坐标 在左下分区
        else if (x > midX && y <= midY){
            if (node.nBL == null) {
                SegTreeNode childBL = new SegTreeNode(midX + 1, y1, x2, midY, 0);
                node.nBL = childBL;
            }
            add(node.nBL, x, y, val);
        }
        // 情况4：新加入的cell的坐标 在右下分区
        else { // x > midX && y > midY
            if (node.nBR == null) {
                SegTreeNode childBR = new SegTreeNode(midX + 1, midY + 1, x2, y2, 0);
                node.nBR = childBR;
            }
            add(node.nBR, x, y, val);
        }
    }
    
    public void update(int x, int y, int val) {
        int difference = val - matrix[x][y];
        matrix[x][y] = val; // 这个别忘了
        
        add(root, x, y, difference);
    }
    
    public int sumRange(int x1, int y1, int x2, int y2) {
        return getSum(root, x1, y1, x2, y2);
    }
    
    private int getSum(SegTreeNode node, int x1, int y1, int x2, int y2) {
        if (x1 == node.x1 && y1 == node.y1 && x2 == node.x2 && y2 == node.y2) {
            return node.sum;
        }
        
        int nx1 = node.x1, ny1 = node.y1;
        int nx2 = node.x2, ny2 = node.y2;
        
        // left到mid算 左子树，mid+1到right算 右子树。水平和竖直方向都遵循这个做法
        int midX = nx1 + (nx2 - nx1) / 2;
        int midY = ny1 + (ny2 - ny1) / 2;
        
        // 对于一个node所代表的 matrix 的4个分区来说，对于要查询的范围(x1,y1,x2,y2)，共有 4 + 4 + 1 = 9 种情况！
        // 整个 (x1,y1,x2,y2) 完全在 node的range的 左上部
        if (x2 <= midX && y2 <= midY) {
            return getSum(node.nTL, x1, y1, x2, y2);
        }
        // 整个 (x1,y1,x2,y2) 完全在 node的 右上部
        else if (x2 <= midX && y1 > midY) {
            return getSum(node.nTR, x1, y1, x2, y2);
        }
        // 整个 (x1,y1,x2,y2) 完全在 node的 左下部
        else if (x1 > midX && y2 <= midY) {
            return getSum(node.nBL, x1, y1, x2, y2);
        }
        // 整个 (x1,y1,x2,y2) 完全在 node的 右下部
        else if (x1 > midX && y1 > midY) {
            return getSum(node.nBR, x1, y1, x2, y2);
        }
        // (x1, y1, x2, y2) 在node的 左上及右上部
        else if (x2 <= midX) { // && y1 <= midY && y2 > midY
            return getSum(node.nTL, x1, y1, x2, midY) + 
                   getSum(node.nTR, x1, midY + 1, x2, y2);
        }
        // (x1, y1, x2, y2) 在node的 左下及右下部
        else if (x1 > midX) { // && y1 <= midY && y2 > midY
            return getSum(node.nBL, x1, y1, x2, midY) + 
                   getSum(node.nBR, x1, midY + 1, x2, y2);
        }        
        // (x1, y1, x2, y2) 在node的 左上及左下部
        else if (y2 <= midY) { // && x1 <= midX && x2 > midX
            return getSum(node.nTL, x1, y1, midX, y2) + 
                   getSum(node.nBL, midX + 1, y1, x2, y2);
        }
        // (x1, y1, x2, y2) 在node的 右上及右下部
        else if (y1 > midY) { // && x1 <= midX && x2 > midX
            return getSum(node.nTR, x1, y1, midX, y2) + 
                   getSum(node.nBR, midX + 1, y1, x2, y2);
        }
        // (x1, y1, x2, y2) 与node的 4个分区 都有交集，注意！不可能相交正好3个分区！
        else { // x1 <= midX && y1 <= midY && x2 > midX && y2 > midY
            return getSum(node.nTL, x1, y1, midX, midY) + 
                   getSum(node.nTR, x1, midY + 1, midX, y2) +
                   getSum(node.nBL, midX + 1, y1, x2, midY) +
                   getSum(node.nBR, midX + 1, midY + 1, x2, y2);
        }
    }
}

public class NumMatrix {
    SegTree sg;

    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            this.sg = null;
            return;
        }
        this.sg = new SegTree(matrix);
    }
    
    public void update(int row, int col, int val) {
        if (this.sg == null) return;
        this.sg.update(row, col, val);
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        if (this.sg == null) return Integer.MIN_VALUE;
        return sg.sumRange(row1, col1, row2, col2);
    }
}
```

## Reference
* [Range Sum Query 2D - Mutable [LeetCode]](https://leetcode.com/problems/range-sum-query-2d-mutable/description/)

{% include links.html %}
