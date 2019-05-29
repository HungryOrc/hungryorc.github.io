---
title: "Max Element in Sliding Window"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_maxElement_inSlidingWindow.html
folder: algorithm
toc: false
---

## Description
Given an integer array A and a sliding window of size K, 
find the maximum value of each window as it slides from left to right.

Assumptions:
* The given array is not null and is not empty
* K >= 1, K <= A.length

### Example
* Input: A = {1, 2, 3, 2, 4, 2, 1}, K = 3
  * Output: the windows are {{1,2,3}, {2,3,2}, {3,2,4}, {2,4,2}, {4,2,1}}, so the maximum values of each K-sized sliding window are [3, 3, 4, 4, 4]

## Solution: 单调栈(单调下降)。两头读写，用Deque实现
自定义一个 element class，每个element包括原数组里的 int element的value，以及原int在int array里的index。对照着原int数组搞一个element数组

然后，对于element数组，从左到右，每新来一个 原数组里的 int element，就：
1. 先看当前deque尾部（右边）的元素的value，是否比cur的value要小，如果是，则用poll last消灭这些元素。
  * 这么做是因为：有cur在，这些元素既比cur小，又在cur的前面，所以这些元素必然永远也翻不了身，不可能成为任何sliding window的max。
2. 然后把cur放在deque的尾部
3. 再看当前deque头部的元素的index，是否超过了sliding window的左界限，如果超过了，就用poll first来消灭掉，
它们超越了window的界限，自然不可能留下
4. 然后把当前deque的头部的元素的value放到result list里去
这一步必须在 当前cur的 index >= k - 1 的时候才能做！ 比如k=3，则到了元素index至少为2的时候，才能凑成长度为k=3的window！

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
没在网上找这题

{% include links.html %}
