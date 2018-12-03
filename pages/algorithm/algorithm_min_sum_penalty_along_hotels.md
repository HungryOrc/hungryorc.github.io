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
  * 注意，这里必须满足：ai - aj <= 200
* 在hotels[i] <= 200 的情况下，上面的递推公式，还要考虑从最初出发点直接到达本hotel的罚款情况
* 初始：dp[0] = (200 - hotels[0])^2
* 要得到dp[n - 1]，但不是要返回它，要返回的是沿路各个停靠hotels。**这些路径点在求最小的总罚款额的时候，就已经可以知道，我们要做的只是把这些节点都记录下来**。我们用一个 list of list 来做这个记录，这个list of list里序号为j的list就是到达 hotel j 时累计罚款额最小的路径，所经停过的（在它前面的那些）hotels

### Complexity
* Time: O(n^2)
* Space: O(n^2)，那个记录路点的 array of list

### Java
代码看起来不短，其实逻辑很简明。有不少代码都是为了把路径存下来，如果只求最小的总罚款额的话，代码会短很多

```java
public class Solution {
    public List<Integer> minTotalPenalty(int[] hotels) {
        List<Integer> result = new ArrayList<>();
        
        // corner cases
        if (hotels == null || hotels.length == 0) {
            return result;
        }
        
        // 查看是否不可能到达终点
        if (hotels[0] > 200) {
            return result;
        }
        int n = hotels.length;
        for (int i = 1; i < n; i++) {
            if (hotels[i] - hotels[i - 1] > 200) {
                return result;
            }
        }
        
        int[] dp = new int[n]; // 到达hotel i 最小的累计罚款额
        List<List<Integer>> checkPoints = new ArrayList<>(); // 到达各个hotels的最优路径
        
        // 初始情况
        dp[0] = (int)Math.pow(200 - hotels[0], 2);
        List<Integer> list = new ArrayList<>();
        list.add(0);
        checkPoints.add(list);
        
        for (int i = 1; i < n; i++) {
            // 给每个 dp[i] 一个初值
            if (hotels[i] <= 200) {
                dp[i] = (int)Math.pow(200 - hotels[i], 2);
            } else {
                dp[i] = Integer.MAX_VALUE; // 这只是为了之后求min penalty at i
            }
            
            int bestPrev = -1;
            int j = 0;
            for (; j < i; j++) {
                int diff = hotels[i] - hotels[j];
                if (diff <= 200) {
                    int update = dp[j] + (int)Math.pow(200 - diff, 2);
                    
                    if (update < dp[i]) {
                        dp[i] = update;
                        bestPrev = j;
                    }
                }
            }
            
            List<Integer> path = new ArrayList<>();
            if (bestPrev != -1) { // 如果当前hotel j 的最优路径是从最初出发点直接到 j
                path.addAll(checkPoints.get(bestPrev));
            }
            path.add(j);
            checkPoints.add(path);
        }
        
        return checkPoints.get(n - 1);
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
