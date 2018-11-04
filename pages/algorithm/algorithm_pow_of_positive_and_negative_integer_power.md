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
借这道题，把所有pow类的题归纳一遍。这里n是正整数，如果n可能是负整数也无所谓，最后一步搞成倒数就行了。

### Method 1
```java
public double pow(double a, int n) {
    if (n == 0) {
        return 1;
    }
    return pow(a, n - 1) * a;
}
```
* Time: O(n)
* Space: O(n)，因为有n层call stack，每层是O(1)的空间消耗

### Method 2
```java
public double pow(double a, int n) {
    if (n == 0) {
        return 1;
    }
    if (n == 1) {
        return a;
    }
    return pow(a, n / 2) * pow(a, n - n / 2); 
}
```
* Time: O(n)，因为 1 + 2 + 4 + 8 + ... 2^(log_2(n)) = 1 + 2 + 4 + ... + n/2 = n
* Space: O(logn)，因为有logn层call stack，每层是O(1)的空间消耗

### Method 3
```java
public double pow(double a, int n) {
    if (n == 0) {
        return 1;
    }
    double half = pow(a, n / 2);
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * a;
    }
}
```
* Time: O(logn)，因为有记忆化
* Space: O(logn)

### Complexity
这一题我们实际做的时候，用最后一种最好的方法，所以复杂度如下：
* Time: O(logn)
* Space: O(logn)

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
