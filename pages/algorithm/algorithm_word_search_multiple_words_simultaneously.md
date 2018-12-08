---
title: "Word Search: Find Max Number of Words Simultaneously in a Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_word_search_multiple_words_simultaneously.html
folder: algorithm
toc: false
---

## Description
给一个矩阵 char[][] board，给一个数组 String[] words，求在board里同时能够最多存在多少个word里面的词。
board和words里面都只有小写字母a到z，没有任何其他char。
“同时存在”是指任何词的任何字母都不能在matrix上重叠。一个词可以在matrix上沿着上下左右四个方向进行延伸。

这题与Word Search II相似。但是Word Search II是在board里每次找words里的一个词。这题是要看board里最多能同时存在words里的几个词。
这题的难点在于 **在找到一个词以后，boolean[][] visited矩阵要把这个找到的词的各个chars的位置标记好；但另一方面，在找一个词的过程中，
它的各个char每次被添加到visited矩阵里以后，都要在适当的时候做back tracking，即从visited矩阵里剔除出去**。

### Example
* Input: String[] dict = {"abc","deh","cfi","eh", "xy", "fi", "f"}
  ```
  char[][] board = {{'a', 'b', 'c'},
                    {'d', 'e', 'f'},
                    {'g', 'h', 'i'}};
  ```
  * Output: 3, for example a max size set is: {"abc", "deh", "fi"}

## Solution: DFS，特殊之处在于 DFS函数里面还有双层for循环！
“DFS函数的内部结构也许看起来疯狂，但其实 只要当前的状态越来越接近 Base Case，就不会无限循环，就没错”

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
