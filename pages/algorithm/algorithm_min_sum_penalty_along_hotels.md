---
title: "Minimum Sum of Penalties along the Hotels"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_min_sum_penalty_along_hotels.html
folder: algorithm
toc: false
---

## Description
一条一维的路程，上面分布了 n 个 hotels。从出发点(不是hotel)开始，一直到终点的hotel。这些hotels距离出发点的距离分别是 a1, a2... an。
每天晚上必须在某个hotel过夜，否则会被orcs劫走。

每天只能最多走200 miles。但是走不到 200 miles会被罚款。penalty等于 (200 - x)^2，其中x是当日前进的距离。注意除了第一天的出发点不是hotel，
每一天的起点和终点都是某些hotels。我们要实现从起点到终点最少的总罚款数额，返回这个最优路线沿线在哪些hotels停靠过（用一个List<Integer>表示）。不关心一共用了多少天。

如果最终无法到达终点，则返回一个空的list。

### Example
* Input: int[] hotels = {50, 150, 250, 300}，即第一个hotel距离出发点50 miles，其余的类推
  * Output: 最优路径：第一天前进到距离出发点150的那个hotel，罚款 (200-(150-0))^2 = 50^2；
  第二天前进到距离最初出发点300的那个hotel，罚款 (200-(300-150))^2 = 50^2。全旅途一共罚款 5000。返回 [1, 3]。

## Solution: DP
这题一看就是要用DP。但是首先要解决最终能否到达终点的问题！不要一开始就想当然地以为一定可以到达

容易想到，**如果任何两个相邻的hotels之间的距离 > 200，则一定无法到达终点**。否则，一定可以到达。所我们要先做这个检测

* int[] dp = new int[n]，其中 dp[i] 表示到达 hotel i（不管哪一天到达），最少的**累计**罚款额
* `dp[i] = min(dp[j] + (200 - (ai - aj))^2)`
  * 0 <= j < i
* 初始：dp[0] = (200 - hotels[0])^2
* 要得到dp[n - 1]，但不是要返回它，要返回的是沿路各个停靠hotels。**这些路径点在求最小的总罚款额的时候，就已经可以知道，我们要做的只是把这些节点都记录下来**。我们用一个 array of list 来做这个记录，这个array里坐标j的list就是到达 hotel j 时累计罚款额最小的路径，所经停过的（在它前面的那些）hotels

### Complexity
* Time: O(n^2)
* Space: O(n^2)，那个记录路点的 array of list

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
