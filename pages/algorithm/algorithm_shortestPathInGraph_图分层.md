---
title: "Shortest Path in a Graph that Has Layers, 图分层"
tags: [algorithm, graph]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortestPathInGraph_图分层.html
folder: algorithm
toc: false
---

## Description：Google onsite 题
给几层点，每两层之间fully connected (比如第一层的任意点和第二层的任意点都**直接**相连， 不相邻的层之间不相连)。
node 有cost，edge 有cost，求第一层的任意点到最后一层的任意点的所有路径中，最小cost是多少

### Example
略

## Solution
从最上面一层开始，每次只做相邻的两层。最后更新终点层的每一个点的cost，最小的那个就是答案了。具体来说如下，比如说有三层：
* 先看第一层到第二层
  * 看第二层里的某一个点，看第一层的每个点到它的cost，找到最小的那个值，就是我们到达第二层的这个点的最小cost
  * 对于第二层里的每个点，都作上述做法，那么就得到每个点的最小cost
* 再看第二层到第三层。此时第二层每个点的cost就设为从第一层到达这个点的最小cost
  * 根据上面一样的方法，得到到达第三层的每个点的最小cost
  * 这些min costs 里的 min 就是从第一层到第三层的 最小cost

### Complexity
* Time: O(|V| + |E|)，因为每个点和每个边都要考察。然后本图的分层的情况不定，所以|V|和|E|的量级关系不定 <=== ？？？？
* Space: O(n) <=== ？？？？

### Java
```java

```

## Reference
网上没看见这题

{% include links.html %}
