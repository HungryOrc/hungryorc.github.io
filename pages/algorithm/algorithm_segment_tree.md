---
title: "Segment Tree"
tags: [algorithm, algorithm_overview, segment_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_segment_tree.html
folder: algorithm
toc: false
---

## Overview
在关注一个range里的min、max、count、sum之类的性质的时候，就可以考虑用 segment tree。但这也不是绝对的。如果 update data 之类的 modify 性质的操作较多，
则用 segment tree 较好。如果modify类的操作较少或者没有，主要只是query类的操作，则可能用一个数组就可以了，还更快捷。

至于 Augment Tree (增强树)，它和 Segment Tree 都是“高级”版的树状结构。不过 Agument Tree 对于 range 类的问题没什么长处，它的增强主要在于对单个 tree node 的增强。

### Complexity
* Time: O(n*logn)
* Space: O(n)


## Implementation

### Java
```java
```

{% include links.html %}
