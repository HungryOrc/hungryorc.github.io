---
title: "Min Number of Rescue Boats"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_min_number_of_rescue_boats.html
folder: algorithm
toc: false
---

## Description
要从一个沉船/小岛上撤离所有的人。所有人的weight放到一个int数组里。所有救生艇的重量承载力是一样的。求最少需要多少条救生艇，才能把所有人都救走

Note:
* 默认参数都valid。包括所有人的weights都 > 0，船的 weight capacity 也 > 0，而且要大于等于最重的那个人的weight。

注意！这题和 cut wood，merge piles of stones，打气球 等可以用DP做的题目的重大区别在于，cut wood 等必须按照一定的顺序来安排元素，而
这题运人没有任何顺序，任何人都可以上任何船。

### Example
* Input: [2,2,3,5,4], limit of each boat is 5
  * Output: 4

## Solution：老老实实用permutation做

这类题不要随便用greedy做！绝大部分的greedy做法在数学上都是错误的！

### Complexity
* Time: O(n * n!)
* Space: O(n)

### Java
```java

```

## Reference
没找到这题

{% include links.html %}
