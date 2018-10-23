---
title: "Shortest Path in a Matrix with Wall Breaking"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortest_path_in_matrix_with_wall_breaking.html
folder: algorithm
toc: false
---

## Description
一个正方形迷宫，边长n，里面1的cell是墙，0的cell是路。每次可以走上下左右四个方向。给定起始点s和终止点t。可以破墙，最多k次。问从s到t的最短路径是多长。一个被破的墙的cell也算距离1，
一个原本就是路点的cell也算距离1。

### Example
* Input:
  * Output:

## Solution
用图论来做这题。所谓的“图”里的每个“点”是这么一个状态：(x, y, b)，意思是破墙b次后到达矩阵里坐标 (x, y) 处。
* 从起始点向外逐层扩散，BFS
* 没遇到墙的地方，和普通的迷宫走路题一样
* 遇到墙的地方，可以破，也可以不破，两种选择都ok，形成两个支路。当然如果k次破墙法术用尽了，那就不能再破了
* 终止条件：首次走到终点坐标，而且破墙次数 <= k。即达到状态 (xt, yt, bt)，其中 bt <= k

那么对于这个图，它的点和边的个数分别为：
* |V| = O(k * n^2)，k 是破墙次数，因为对于矩阵里的每个坐标点，都最多可能经过k次破墙达到它，那么对于整个图来说，所有的状态种类就一共有 k * n^2 种
* |E| = O(4 * k * n^2)，因为每个“状态”都可以向4个方向“走”，走到上下左右的另外4个状态，那么就相当于“边”的个数是“点”的个数的四倍

### Complexity
* Time: O(|V| + |E|)，因为worse情况可能需要全“图”遍历     <===== 对么？？
* Space: O(|V|)，因为BFS的queue里最多装|V|量级的状态点    <==== 对么？？

### Java
```java

```

## Reference
* 网上没有找到这题

{% include links.html %}
