---
title: "Kth Smallest Element in a Matrix that is Sorted Both by Row and Column"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_kthSmallest_inMatrixSortedByRowAndCol.html4
folder: algorithm
toc: false
---

## Description
Given a n * m matrix where each of the rows and columns are sorted in ascending order, 
find the kth smallest element in the matrix.
* It is the kth smallest element in the sorted order, not the kth distinct element.
* You may assume k is always valid, 1 ≤ k ≤ n * m

注意这几个误区：
1. 并不一定沿着每一条对角线，从右上方到左下方越来越大。反例如下：
   ```
   1  2  3
   2  5  6
   2  7  9
   ```
2. 并不一定 更靠右的(右上到左下的)对角线上的任何数 都比 更靠左的对角线上的任何数要大。反例如下：
   ```
   1  3  5
   6  7  12
   11 14 14
   ```
   上面的矩阵里，第一排最右边的5，就比第二排第一个的6要小

### Example
* Input: k = 8, matrix is:
  ```
  [ 1,  5,  9]
  [10, 11, 13]
  [12, 13, 15]
  ```
  * Output: 13

## Solution 1： Priority Queue (Min Heap) with Helper Class，面试时用这个方法来写
首先把最左上角的元素放到PQ里去。之后不断地 pop PQ，得到当前最小，然后把当前最小的 cell 的右边和下面的2个cell 放到 PQ 里去。

**每个cell的坐标和value都要封装到一个 helper class "Cell" 里去，然后作为一个整体 放到 PQ 里去**

### Complexity
* Time: O(k logk)，这是 PQ 带来的时间代价，k 是要找k个数的k，不是所有数的个数n
* Space: O(k + n * n)，其中 n * n 是在解题过程里，PQ 里可能达到的装东西的量级

### Java
```java
// helper class
class Cell {
    int x, y;
    int value;
  
    public Cell(int x, int y, int value) {
        this.x = x;
        this.y = y;
        this.value = value;
    }
}

public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        // ignore sanity checks
        
        int n = matrix.length;
        int m = matrix[0].length;
    
        PriorityQueue<Cell> minHeap = new PriorityQueue<>(k, new Comparator<Cell>() {
            @Override
            public int compare(Cell c1, Cell c2) {
                if (c1.value == c2.value) {
                    return 0;
                }
                return c1.value > c2.value ? 1 : -1;
            }
        });
      
        minHeap.offer(new Cell(0, 0, matrix[0][0]));
    
        // 需要一个visited矩阵！因为一个cell要延伸到其右边和其下方的2个cells，那么不同的cell可能就会延伸到同一个cell
        boolean visited[][] = new boolean[n][m];
        visited[0][0] = true;
    
        // poll out from the min heap for k-1 times, including the initial cell at [0][0],
        // so the one left at the top of the min heap will be our target: the kth smallest element,
        // at that time we can get it via peek()
        // 注意，在此过程中，poll了 k - 1 次，而offer的次数很有可能是大于k-1次的！！！因为poll一个cell可能就
        // 要接下来再offer与它相邻的2个或1个或0个cells
        // 当然，还有一种可能是，整个矩阵都offer到heap里了，但poll还没有poll到k-1次，这个情况，下面的代码也是包含了的
        for (int count = 1; count <= k - 1; count++) {
            Cell curMinCell = minHeap.poll();
            int curX = curMinCell.x;
            int curY = curMinCell.y;
            int curValue = curMinCell.value;
      
            if (curX + 1 < n && !visited[curX + 1][curY]) {
                minHeap.offer(new Cell(curX + 1, curY, matrix[curX + 1][curY]));
                visited[curX + 1][curY] = true;
            }
            if (curY + 1 < m && !visited[curX][curY + 1]) {
                minHeap.offer(new Cell(curX, curY + 1, matrix[curX][curY + 1]));
                visited[curX][curY + 1] = true;
            }
        }
        return minHeap.peek().value; // 前面一共poll了k-1次，到了这里再peek就是peek到第k个数了
    }  
}
```

## Solution 2：Binary Search。最左上角一定是global min，最右下角一定是global max，思路挺巧妙的，但不好写，仅供参考
Ref: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/

这个方法的速度我感觉应该比前面的慢，但实测速度并不比前面的方法慢，不知道为什么

### Complexity
* Time: O(log(nm) * (nlogm))，其中n是矩阵的行数，m是列数 <=== 对么 ？？？？
* Space: O(1) <=== 对么 ？？？？

### Java
```java
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length; // rows
        int m = matrix[0].length; // cols
   
        int min = matrix[0][0];
        int max = matrix[n - 1][m - 1];
        
        // 对每一个mid值，用二分法计算 matrix里的每一行，有多少个数 <= mid值
        // 另一种同理的做法：从左到右，逐列，找这一列里 <= mid 的数有多少个
        int count = 0;
        while(min < max) {
            int mid = min + (max - min) / 2;
            count = 0; 
           
            // 从上到下，逐行 找这一行里 <= mid 的数有多少个
            for(int i = 0; i < n; i++) {
                count += numOfSmallerOrEqual(matrix[i], mid);
            }

            // 对于当前的mid值来说，count算出来了。现在把count和k做对比
            if (count < k) {
                min = mid + 1;
            }
            // count == k 的话，不要 mid-1，因为可能有多个数都(并列)等于mid
            else if (count >= k) {
                max = mid;
            }
            
            /* 注意！
            count == k 时，不要马上 return mid，而是非要等到最后 high 和 low重合再return
            这是因为，mid这个数只是一个标杆，一般来说 不会是给的数组里的任何一个元素 */
        }
        
        System.out.println("count: " + count);
        if (count < k ) return max;
        return min;
    }
    
    // int[] nums is an un-empty sorted array
    private int numOfSmallerOrEqual(int[] nums, int target) {
        int n = nums.length;
        int l = 0, r = n - 1;
        
        while (l + 1 < r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] <= target) {
                l = mid;
            } else { // if (nums[mid] > target)
                r = mid - 1;
            }
        }
        
        // 最后 l 应该是在 r 的左边一位
        if (nums[r] <= target) {
            return r + 1;
        }
        if (nums[l] <= target) {
            return l + 1;
        }
        // 别忘了这种情况！
        // 如果l和r所指向的数都 >= mid，则意味着本数组里没有 < mid 的数！
        return 0;
    }
}
```

## Reference
* [Kth Smallest Element in a Sorted Matrix [LintCode]](https://www.lintcode.com/problem/kth-smallest-element-in-a-sorted-matrix/description)

{% include links.html %}
