---
title: "KMP (Knuth Morris Pratt) Pattern Searching Algorithm"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_KMP.html
folder: algorithm
toc: false
---

## Overview
https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm

KMP 是用 O(n + m) 时间完成在一个 String `source` (长度n) 里寻找一个 Sub String `word` (长度m) 的算法。它的思想内核是：所有信息都已经包含在
`word`里了。我们在`word`里的任何一个位置发生匹配失败的时候，都能在`word`里的某个位置重新开始比对，而未必需要总是回到`word`的第一个char，
那么到底回到哪一位，我们是可以从`word`本身完全知道的，和任何`source`都没有关系

那么对于`word`里的每一位，匹配失败后回到哪一位去继续找，这些信息我们用一个所谓的 Rollback Array `T` 来表示。举例：
```
index i  0	 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
word[i]  P	 A  R  T  I  C  I  P  A  T  E     I  N     P  A  R  A  C  H  U  T  E
T[i]    -1  0  0  0  0  0  0  0  1  2  0  0  0  0  0  0  1  2  3  0  0  0  0  0
```


### Complexity
* Time: 
* Space: 

## Implementation

```java

```

## Reference

{% include links.html %}
