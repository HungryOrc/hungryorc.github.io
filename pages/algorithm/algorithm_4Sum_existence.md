---
title: "Four Sum, Check If There Exists a Quadruplet that Sum up to the Target Number"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_4Sum_existence.html                               
folder: algorithm
toc: false
---

## Description
Determine if there exists a set of four elements in a given array that sum to the given target number.

Note:
* The given array is not null and has length of at least 4
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: {1, 2, 2, 3, 4}, target = 9
  * Output: True. 1 + 2 + 2 + 4 = 9

## Solution：把4个数化为2个Pair！然后用瞄准一个pair的loop来实现模拟两个pairs！

用 HashMap 以及 双层 for loop

把想象中的4个数分为2个pair，这4个数就分别是 p1L, p1R, p2L, p2R. 它们的和需要等于特定的target。
然而还有一个重要的要求，就是 p2L 的index 必须 > p1R 的index

### Complexity
* Time: O(n^2)
* Space: O(n)，map <=== 对么 ？？？？

### Java
```java

```

## Reference
[3Sum [LeetCode]](https://leetcode.com/problems/3sum/description/)

{% include links.html %}
