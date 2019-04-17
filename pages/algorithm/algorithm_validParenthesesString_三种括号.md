---
title: "Valid Parentheses"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validParenthesesString_三种括号.html
folder: algorithm
toc: false
---

## Description
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:
* Open brackets must be closed by the same type of brackets.
* Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

### Example
* Input: `([)]`
  * Output: false
* Input: `([)[]]((()))`
  * Output: true

## Solution：速度前1%
简明方法，从左到右，用一个stack

### Complexity
* Time: O(n)
* Space: O(n)，stack size

### Java
```java
class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() == 0) {
            return true;
        }
        
        Stack<Character> stack = new Stack<>();
        
        for (char c : s.toCharArray()) {
            if (c == ')') {
                if (stack.isEmpty() || stack.peek() != '(') {
                    return false;
                }
                stack.pop();
            } else if (c == ']') {
                if (stack.isEmpty() || stack.peek() != '[') {
                    return false;
                }
                stack.pop();
            } else if (c == '}') {
                if (stack.isEmpty() || stack.peek() != '{') {
                    return false;
                }
                stack.pop();
            } else {
                stack.push(c);
            }
        }
        
        if (!stack.isEmpty()) {
            return false;
        }
        return true;
    }
}
```

## Reference
* [Valid Parentheses [LeetCode]](https://leetcode.com/problems/valid-parentheses/description/)

{% include links.html %}
