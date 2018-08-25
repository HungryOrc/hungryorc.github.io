---
title: "Tower of Hanoi"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_tower_of_hanoi.html
folder: algorithm
toc: false
---

## Description
You are given an integer array `nums` and you have to return a new `counts` array. 
In the `counts` array, `counts[i]` is the number of smaller elements to the right of `nums[i]`.

## Example

* Input: nums = [5, 2, 6, 1]
  * Output: counts = [2, 1, 1, 0]

## Solution


### Complexity

* Time: O(2^n)
  * T(n) = 2 * T(n-1) + 1, so you can get the answer...
* Space: O(n)

### Java
This solution is a bit different to the requirement of the question above, but the overall methodology is the same.

```java

```


## Reference
* [Tower of Hanoi - LintCode](https://lintcode.com/problem/tower-of-hanoi/description)

{% include links.html %}
