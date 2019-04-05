---
title: "Divide Two Integers without Multiplication and Division"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_divide_two_integers_without_division.html
folder: algorithm
toc: false
---

## Description：这题感觉有些无聊，但我的做法速度很慢，回头可看看人家怎么做的

Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

Note:
* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. 
For the purpose of this problem, assume that your function returns 2^31 − 1 when the division result overflows.


### Example
* Input: dividend = 10, divisor = 3
  * Output: 3
* Input: dividend = -7, divisor = 3
  * Output: -2

## Solution：我自己的方法，用减法来做除法

### Complexity
* Time: O(n)，n是dividend的大小，这个速度确实是比较慢的
* Space: O(1)

### Java
```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == 0) {
            return 0;
        }
        if (divisor == 1) {
            return dividend;
        }
        if (divisor == -1) {
            if (dividend == Integer.MIN_VALUE) { // 缘于这题的题意的特殊处理，好无聊
                return Integer.MAX_VALUE;
            }
            return 0 - dividend;
        }
        
        int count = 0;
        
        boolean negative = false;
        if ((dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0)) {
            negative = true;
        }
        
        long divid = Math.abs((long)dividend);
        long divis = Math.abs((long)divisor);
        
        while (divis <= divid) {
            divid -= divis;
            count++;
        }
        
        if (negative) {
            return 0 - count;
        }
        return count;
    }
}
```

## Reference
* [Divide Two Integers [LeetCode]](https://leetcode.com/problems/divide-two-integers/description/)

{% include links.html %}
