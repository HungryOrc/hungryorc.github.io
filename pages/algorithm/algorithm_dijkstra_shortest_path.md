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
Dijkastra's Algorithm 是一种 Graph 上的算法，用于计算 从图上的某1点出发，到其他所有点的分别的最短距离
* 所采用的主要数据结构是 Priority Queue (Min Heap)。
* 当起始点到所有点的距离都算完的时候，这个 priority queue 应该是空的
* Cost(source to cur node) = Cost(source to parent node) + Cost(parent to cur node)
* Generation / Expansion rule for the nodes in this algorithm
* **一个node会被放入到PQ里一次或多次，但当一个node被从PQ里第一次弹出来的时候，它到source node 的最短距离就定了，就是弹出来时候的距离，之后不管它还有多少次被弹出来，距离都不可能比这个第一次被弹出来的时候更短了**。解释：比如和你血缘最近的是亲父母和亲子女，从他们开始向外扩散，得到的那些人和你的血缘关系递减，到了邻居老王的儿子狗剩的时候，血缘关系应该是极为稀薄了，这就叫通过一个很长的路径第一次到达一个node。但是如果另一方面，从你的亲儿子那里出发，其实早就有一条非常短的边直接连到了狗剩（比如狗剩是你儿子的亲哥），那你和狗剩之间的距离其实是比你以为的要短得多。换句话说，从你最紧密的人们出发的BFS，得到的次一级紧密的人(比如狗剩)和你的关系紧密度 T1，是必然要高于或等于 任何和你不那么紧密的人的BFS所得到的狗剩和你的关系紧密度 T2 的

以下图为例，每个 `Nx` 表示 `Node x`，每个 `--y--` 表示这个是一条边，这条边的长度是 `y`（两个方向走都是距离y）。
```
N6 --4-- N4 --9-- N5 
         |        |   \
         1        2     1
         |        |       \
         N3 --3-- N2 --1-- N1

```
设 N4 为起始点，即要求 N4 到图上所有其他点的最短距离分别是多少。步骤：
* 把 (N4, 0) 放到 priority queue 里去，其中后面那个int 0 表示从source node（即N4）开始到当前node的距离（可能是最终的最短距离，也可能只是计算结果中途的疑似最短距离）。在 PQ 里面，也是靠这个距离值进行排序的。当前 PQ 里距离最小的那个 node，会被排在 PR 的top。
* 在 PQ 里，把top node 即 N4 弹出来。然后更新 N4 的所有邻居的距离，然后把这些邻居都放到 PQ 里去。
  * 所以这些会被放到 PQ 里去：(N6, 4), (N3, 1), (N5, 9)
  * 然后 (N3, 1) 会自然被置于 PQ 的顶端
* 在 PQ 里，再次 pop top，N3 被弹出来了。


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
