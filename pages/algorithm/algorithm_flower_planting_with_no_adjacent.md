---
title: "Flower Planting With No Adjacent"
tags: [algorithm, greedy, dfs]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_flower_planting_with_no_adjacent.html
folder: algorithm
toc: false
---

## Description
这题LeetCode标的是easy，但其实 median+。

You have N gardens, labelled 1 to N.  In each garden, you want to plant one of 4 types of flowers.

paths[i] = [x, y] describes the existence of a bidirectional path from garden x to garden y.

Also, there is no garden that has more than 3 paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return any such a choice as an array answer, where answer[i] is the type of flower planted in the (i+1)-th garden.  The flower types are denoted 1, 2, 3, or 4.  It is guaranteed an answer exists.

Note:
* 1 <= N <= 10000
* 0 <= paths.size <= 20000
* No garden has 4 or more paths coming into or leaving it. 就是说对于每个花园，与之连接的paths的个数 小于等于3，这题里的paths都是双向的，所以不存在 入度 和 出度 的说法
* It is guaranteed an answer exists.

### Example
* Input: N = 3, paths = [[1,2],[2,3],[3,1]]
  * Output: [1,2,3]
* Input: N = 4, paths = [[1,2],[3,4]]
  * Output: [1,2,1,2]
* Input: N = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
  * Output: [1,2,3,4]

## Solution 1: Greedy，速度前 50%
颜色一共有四种，但每一个花园即每一个vertex的edge的个数最多是三。

所以，**其实无论怎么涂色，都不会撞车，即要涂某一个花园的时候，看它周围的所有与之相连的花园（个数 <= 3），选一个它们都没有用到的颜色，给自己涂色就行了。而因为 4 > 3 所以我们永远可以找到这样的颜色。这种可行性，与之前的涂色顺序无关，与之前的选用颜色无关，与当前为自己的选用颜色也无关！这就是这题可以用Greedy的元气所在。哦也**

### Complexity
* Time: O(|E|)，要考察每一条边 <==== 对么 ？？？？
* Space: O(|E|)，map的size <==== 对么 ？？？？

### Java
```java
class Solution {
    public int[] gardenNoAdj(int n, int[][] paths) {
        Map<Integer, Set<Integer>> map = new HashMap<>();
        int[] result = new int[n];	

        for (int i = 0; i < n; i++) {
            map.put(i, new HashSet<>());
        }

        for (int[] path : paths) {
            // gardens是从1开始编号的，按照题意
            int v1 = path[0] - 1;
            int v2 = path[1] - 1;
            
            map.get(v1).add(v2);
            map.get(v2).add(v1);
        }

        for (int i = 0; i < n; i++) {
            boolean[] used = new boolean[5]; // 0 means no color, 1 ~ 4 means four colors

            for (int neighbor : map.get(i)) {
                used[result[neighbor]] = true;
            }

            for (int color = 1; color <= 4; color++) {
                if (!used[color]) {
                    result[i] = color;
                    break;
                }
            }
        }
        return result;
    }
}
```

## Reference
* [Flower Planting With No Adjacent [LeetCode]](https://leetcode.com/problems/flower-planting-with-no-adjacent/description/)

{% include links.html %}
