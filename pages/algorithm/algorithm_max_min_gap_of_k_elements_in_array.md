---
title: "Max Min Gap of K Elements in An Array"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_max_min_gap_of_k_elements_in_array.html
folder: algorithm
toc: false
---

## Description
一个int array，每两个element之间的距离 定义为 它们两的值之差。那么k个element之间的最短间距，就是看这k个里面逐个的距离哪个最小了。
问：在array里随便找k个元素，它们的最短间距的最大值是多少

### 我自己想到的 Follow up
如果k个点是在一个平面上，而非直线上，那怎么快速计算最短间距？

还没想到好的办法。难道用geo hash？分为多个小方块来算最短间距？

### Example
* Input: [2, 3, 4, 7], k = 3
  * Output: 2。这(k=)3个点分别取在 2，4，7，这是它们可能实现的两两之间隔得最开的方案。所以最大的的最小间距是2

## Solution：高级的二分法运用
相当于在数组里随便pick k 个点，这k个点两两之间要尽量隔开得越远越好。那么自然数组的两个端点是要选的。然后中间再选 k - 2 个点。
到了这里就和锯木头的那题基本一样了：**一个长木头上有一些分段标记，必须锯在这些分段标记上**。

关键点：用二分法来不断接近和摸索最小间距的最大值

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
