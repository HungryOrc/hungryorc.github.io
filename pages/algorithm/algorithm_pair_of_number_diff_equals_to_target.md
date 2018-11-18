---
title: "Find a Pair of Numbers in the Array, whose Difference Equals to the Target"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_pair_of_number_diff_equals_to_target.html
folder: algorithm
toc: false
---

## Description
Given a sorted array A, find a pair (i, j) such that A[j] - A[i] is identical to a target number(i != j).

* If there does not exist such pair, return a zero length array.
* The given array is not null and has length of at least 2.
* 如果有多个pair符合条件，也只返回一个pair就行

### Example
* Input: A = {1, 2, 3, 6, 9}, target = 2
  * Output: return {0, 2} since A[2] - A[0] == 2
* Input: A = {1, 2, 3, 6, 9}, target = -2
  * Output: return {2, 0} since A[0] - A[2] == -2

## Solution
下面的hashmap的做法，适用于原数组sorted or unsorted。
* 对于
下面的hashmap的做法，适用于原数组sorted or unsorted。

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
