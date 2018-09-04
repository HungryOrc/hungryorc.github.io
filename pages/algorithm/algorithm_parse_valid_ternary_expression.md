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
```
Moving N dishes from A to C = Moving N-1 dishes from A to B, with the help of C +
                              Moving 1 dish from A to C +
			      Moving N-1 dishes from B to C, with the help of A
```

### Complexity
* Time: O(2^n)
  * T(n) = 2 * T(n-1) + 1, so you can get the answer...
* Space: O(n)

### Java
This solution is a bit different to the requirement of the question above, but the overall methodology is the same.

```java

```

## Reference
* [Ternary Expression Parser [LeetCode]](https://leetcode.com/problems/ternary-expression-parser/description/)

{% include links.html %}
