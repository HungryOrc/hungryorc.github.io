---
title: "Union Find"
tags: [algorithm, algorithm_overview, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_union_find.html
folder: algorithm
toc: false
---

## Overview
A set of objects, represented by integers 0 to n-1, without loss of generality. You can:
* Connect any two of them to be in the same group, using `void union(int a, int b)`
* Query whether any two of them are in the same group, using `boolean find(int a, int b)`

## Implementation Stage 1: Staight Forward Style
Maitain an int array of size n, array[i] is the group number of the i-th object, 
so if object a and object b are in the same group, then array[a] == array[b], vise versa.

### Java
```java

```

### Complexity
* Time: 
  * Union: O(n), for one union operation, it might need to update O(n) number of elements in the int array，原因是，比如a和b交合，但是a和b后面可能都连着一大伙呢
  * Find: O(1)
* Space: O(n), the size of the int array


{% include links.html %}
