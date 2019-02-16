---
title: "Extend Two Strings and Check Their Differences"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_extend_two_strings_and_check_differences.html
folder: algorithm
toc: false
---

## Description
Given 2 strings which has the same initial length, and they will also have the same length after you extend them in the following way:
"2a3b1c" -> "aabbbc", "1a2b3d" -> "abbccc". Return the count of different digits between the 2 extended strings.

Note:
* The numbers can be more than 1 digit


### Example
* Input: "2a3b1c" and "1a2b3c"
  * Output: 3, since the extended strings are: "a(a)b(bb)c" and "a(b)b(cc)c"。用括号标出来了不同的digits。

## Solution
这题看起来简单，先把两个Strings都展开，再一位一位比就行了。
但是如果里面的数字很大的话，就有很大的优化的空间。比如给的两个Strings是 20000a 和 999b1c，那么其实就不必先展开再一位一位比了

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
