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

### Complexity
* Time: 平均情况下 O(n*logn)
* Space: 平均情况下 O(logn)
  
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
