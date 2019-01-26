---
title: "String Equality with Backspaces"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_string_equality_with_backspaces.html
folder: algorithm
toc: false
---

## Description
2个Strings，长度未知，它们里面的任何位置都有可能有任意个 backspace，每个backspace意味着它前面的一个char要被删掉。我们用 '\' 来代表一个backspace。比如：
* "ab\c" = "ac"
* "abb\c\\\" = ""
* "ab\\\" = invalid string
判断这2个Strings在消掉了所有的backspaces以后，是否相等

注意，这些Strings **有可能 invalid**

要求：Space O(1)。如果没这个要求，就很简单，从左往右walk through 两个Strings就行了。有了这个要求就麻烦点

### Example
* Input: "ab\c" and "ac"
  * Output: true，they are equal

## Solution：从右往左搞，两个Strings每个上面一个指针
要满足 O(1) 的空间，关键在于想到：**从右往左搞！** 很多古灵精怪的题目或者要求，反过来(比如从右往左)搞往往能收奇效

### Complexity
* Time: O(len1 + len2), len1 and len2 are the lengths of the 2 strings
* Space: O(1)

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
