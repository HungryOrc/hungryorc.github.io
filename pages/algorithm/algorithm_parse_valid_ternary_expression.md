---
title: "Parse a Valid Ternary Expression"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_parse_valid_ternary_expression.html
folder: algorithm
toc: false
---

## Description
Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. 
You can always assume that the given expression is valid and only consists of digits 0-9, ?, :, T and F 
(T and F represent True and False respectively).
* The length of the given string can be very long.
* Each number will contain only one digit.
* The conditional expressions group right-to-left (as usual in most languages).
* The condition will always be either T or F. That is, the condition will never be a digit.
* The result of the expression will always evaluate to either a digit 0-9, T or F.

### Example
* Input: Input: "T?2:3"
  * Output: "2"
* Input: "F?1:T?4:5"
  * Output: "4"
  * The conditional expressions group right-to-left, it is read/evaluated as:
    ```
       "(F ? 1 : (T ? 4 : 5))"                   "(F ? 1 : (T ? 4 : 5))"
    -> "(F ? 1 : 4)"                 or       -> "(T ? 4 : 5)"
    -> "4"                                    -> "4"
    ```
* Input: "T?T?F:5:3"
  * Output: "F"
  * The conditional expressions group right-to-left, it is read/evaluated as:
    ```
       "(T ? (T ? F : 5) : 3)"                   "(T ? (T ? F : 5) : 3)"
    -> "(T ? F : 3)"                 or       -> "(T ? F : 5)"
    -> "F"                                    -> "F"
    ```

## Solution


步骤：
* 从 given String (设为`s`) 的最右边的char开始，向左走。用一个stack来装这些chars，那么越靠后的char就越接近栈底
* 遇到`?`就：
  * 从stack里pop出来两个数，首先pop出来的就是`?`之后的第一个char (call it `second char`)，然后pop出来的就是`?`之后紧贴着的那个`:`之后的那个char (call it `third char`)
  * 在`s`里再往左看一个char，这个char就是`?`左边紧贴着的那个char (call it `first char`)
  * 有了这3个char，就可以推出这个5-char组合的值，要么是 second char，要么是 third char
  * 把这个结果push回到stack里去
* 继续在 `s` 里往左走，直到 `s` 的左边第一个char

### Complexity
* Time: O(n)
* Space: O(n), for the stack

### Java
```java

```

## Reference
* [Ternary Expression Parser [LeetCode]](https://leetcode.com/problems/ternary-expression-parser/description/)

{% include links.html %}
