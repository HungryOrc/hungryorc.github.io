---
title: "Dijkstra's Shortest Path Algorithm (One Node to All other Nodes in Graph)"
tags: [algorithm, algorithm_overview]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_dijkstra_shortest_path.html
folder: algorithm
toc: false
---

## Overview
Dijkastra's Algorithm 是一种 Graph 上的算法，用于计算 从图上的某1点出发，到其他所有点的分别的最短距离。以下图为例，
每个 `Nx` 表示 `Node x`，每个 `--y--` 表示这个是一条边，这条边的长度是 `y`（两个方向走都是距离y）。
```
N6 --4-- N4 --9-- N5 
         |        |   \
         1        2     1
         |        |       \
         N3 --3-- N2 --1-- N1

```

### Complexity
* Time: O(？？？)
* Space: O(？？？)
  
## Implementation

```java
public class Solution {
    public int[] quickSort(int[] array) {
        if (array == null || array.length <= 1) {
            return array;
        }
        
        quickSort(array, 0, array.length - 1);
        return array;
    }
    
    private void quickSort(int[] array, int start, int end) {
        if (start >= end) {
            return;
        }
        
        int left = start, right = end;
        int pivot = array[start + (end - start) / 2];

        while (left <= right) {
            if (array[left] < pivot) {
                left ++;
            } else if (array[right] > pivot) {
                right --;
            } else {
                swap(array, left++, right--);
            }
        }
        // 特别注意，while 循环结束以后，left 和 right 的关系是 right + 1 = left 
        // 即 right 在 left 的左边一位
        <=== 是么 ？？？ 有没有可能他们两中间间隔一位 ？？？因为在left==right的时候还可能进行left++且right--的操作！！！
        
        
        // 这里没有判断左右index的大小关系，下一个recursion里的头部会做
        quickSort(array, start, right);
        quickSort(array, left, end);
    }
    
    private void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
```

## Reference
* [Quick Sort [LaiCode]](https://app.laicode.io/app/problem/10)

{% include links.html %}
