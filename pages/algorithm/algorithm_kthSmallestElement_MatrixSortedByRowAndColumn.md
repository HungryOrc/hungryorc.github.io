---
title: "Kth Smallest Element in a Matrix that is Sorted Both by Row and Column"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_kthSmallestElement_MatrixSortedByRowAndColumn.html4
folder: algorithm
toc: false
---

## Description
Given a n * m matrix where each of the rows and columns are sorted in ascending order, 
find the kth smallest element in the matrix.
* It is the kth smallest element in the sorted order, not the kth distinct element.
* You may assume k is always valid, 1 ≤ k ≤ n * m

### Example
* Input: k = 8, matrix = [[ 1,  5,  9],
                          [10, 11, 13],
                          [12, 13, 15]]
  * Output: 13

## Solution
注意：
1. 并不一定沿着每一条对角线，从右上方到左下方越来越大。反例如下：
   1  2  3
   2  5  6
   2  7  9
2. 并不一定靠右的对角线上的任何数都比靠左的对角线上的任何数要大。反例如下：
   1  3  5
   6  7  12
   11 14 14
   上面第一排最右边的5，就比第二排第一个的6要小

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [也许在lintcode上 [LintCode]](网址放在这里)

{% include links.html %}
