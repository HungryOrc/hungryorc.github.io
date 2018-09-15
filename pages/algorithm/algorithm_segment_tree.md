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
则用 segment tree较好。如果modify类的操作较少或者没有，主要只是query类的操作，则可能用一个数组就可以了，还更快捷。

对于Segment Tree，它的每个TreeNode上一定会存一个Range，比如[int start, int end]，即它和它的所有子孙Nodes所处的Range。但是SegmentTreeNode上不会再存一个传统意义上的value，比如说一个SegmentTreeNode上存的Range是[1, 4]，那么它上面不会再存一个所谓的[1, 4]之间的int value，事实上[1, 4]本身就是这个SegmentTreeNode的**value**！

除了Range之外，SegmentTreeNode上还会存至少一个别的值，这些值也不是传统意义上的value，而是与Range相关的某种(甚至多种)属性，比如当前Range里的min、max、sum、count(这个count的意思是当前Range里有几个数据，虽然这些数据本身反而不出现在当前的TreeNode上)之类。

至于 Augment Tree (增强树)，它和 Segment Tree 都是“高级”版的树状结构。不过 Agument Tree 对于 range 类的问题没什么长处，它的增强主要在于对单个 tree node 的增强。

### Complexity
* Time: O(n*logn)
* Space: O(n)


## Implementation 1: 每个TreeNode上存的是当前Range里的TreeNodes的个数count


### Java
```java
class SegmentTreeNode {
    int start, end; // range
    int
}
```

{% include links.html %}
