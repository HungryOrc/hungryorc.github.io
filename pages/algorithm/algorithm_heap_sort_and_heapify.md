---
title: "Heap Sort"
tags: [algorithm, algorithm_overview]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_heap_sort_and_heapify.html
folder: algorithm
toc: false
---

## Overview
A very good video for Heap Sort: [Heap Sort [GeeksforGeeks]](https://www.youtube.com/watch?v=MtQL_ll5KhQ)

Heap Sort的步骤：
* 要sort的array本身已经是一个array，即它已经满足heap必须是complete tree的性质（即tree nodes都要优先铺满左边）。现在这个没有sort的array可以说是一个乱序的heap，我们要把这个array从小到大sort的话，要借助max heap（要从大到小排序的话，用min heap比较方便）：
* 首先要把这个乱序heap变成一个max heap。具体来说，就是对数组里 index = n / 2 - 1（即heap里从下往上的倒数第二层）一直到 index = 0 的各个元素 进行max heapify。每次max heapify 的时间复杂度是 O(logn)。（要建立min heap，就搞min heapify。要建立max heap，就搞max heapify）
  * **Heapify procedure can be applied to a node only if its children nodes are already heapified! That's why heapify must be performed in the bottom up order.**
* 搞完这些之后，max heap已经建好了。heap顶部元素即目前数组的第一个元素就是整个数组的最大元素了。
* 把max heap顶部的元素swap到数组的尾部（index = n - 1），即heap的最下一层最靠右的元素。然后对新swap到顶部的元素进行max heapify，在0到n-2的index范围内。
* 再把新的max元素swap到heap末端，再重复上面的步骤

### Complexity
* Time: O(nlogn)，每次 heapify 的时间是 O(logn)
* Space: O(logn)，因为call stack的层数是height of tree，而heap的层数一定是logn <=== 对么 ？？？

## Implementation
Ref: https://www.geeksforgeeks.org/heap-sort/

### Java
```java

```

## Reference
* [Heap Sort Tutorial [GeeksforGeeks]](https://www.geeksforgeeks.org/heap-sort/)
* [Heap Sort Video [GeeksforGeeks]](https://www.youtube.com/watch?v=MtQL_ll5KhQ)

{% include links.html %}
