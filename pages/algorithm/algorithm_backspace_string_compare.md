---
title: "Backspace String Compare"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backspace_string_compare.html
folder: algorithm
toc: false
---

## Description
Given two strings S and T, return if they are equal when both are typed into empty text editors. # means a backspace character.
* 1 <= S.length <= 200
* 1 <= T.length <= 200
* S and T only contain lowercase letters and '#' characters.
Can you solve it in O(N) time and O(1) space?

### Example
* Input: S = "ab#c", T = "ad#c"
  * Output: True
* Input: S = "ab##cc#d", T = "cd"
  * Output: True

## Solution
如果不要求in place解决，那么非常简单。如果要求in place，则费不少功夫，窍门在于：**从后往前**撸string！

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [Backspace String Compare [LeetCode]](https://leetcode.com/problems/backspace-string-compare/description/)

{% include links.html %}
