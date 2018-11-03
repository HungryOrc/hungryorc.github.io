---
title: "Power(x, n), n can be Positive or Negative Integer"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_pow_of_positive_and_negative_integer_power.html
folder: algorithm
toc: false
---

## Description
Implement pow(x, n), which calculates x raised to the power n (x^n).
* -100.0 < x < 100.0
* n is a 32-bit signed integer, within the range [−2^31, 2^31 − 1]

### Example
* Input: 2.00000, 10
  * Output: 1024.00000
* Input: 2.10000, 3
  * Output: 9.26100
* Input: 2.00000, -2
  * Output: 0.25000

## Solution
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    public double myPow(double x, int n) {
        if (x == 0) {
            return 0;
        }
        if (n == 0) {
            return 1;
        }
        
        boolean negative = false;
        if (n < 0) {
            negative = true;
        }
        n = Math.abs(n);
        
        double result = calcPow(x, n);
        if (negative) {
            return 1 / result;
        }
        return result;
    }
    
    private double calcPow(double x, int n) {
        if (n == 0) {
            return 1;
        }
        if (n == 1) {
            return x;
        }
        
        double half = calcPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
}
```

## Reference
* [Pow(x, n) [LeetCode]](https://leetcode.com/problems/powx-n/description/)

{% include links.html %}
