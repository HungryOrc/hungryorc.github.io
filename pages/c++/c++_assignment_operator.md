---
title: "Assignment Operator (=) in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_assignment_operator.html
folder: c++
toc: false
---

## = 赋予的是 Value, 而非 Reference

```c++
int a = 5;
int b = 7;
a = b; // a = 7, b = 7
b = 1; // a = 7, b = 1
```
注意 `a = b` 只是把 b 当时的 value 给了 a, 并不是把 b 的 reference 给了 a. 即 a 和 b 还是指向两个不同的变量. 所以 b 的值在将来的变化, 对 a 没有影响.

## 连缀使用

```c++
x = y = z = 5; // x = 5, y = 5, z = 5,
```

## 作为别的表达式的一部分

```c++
x = 2 + （y = 5）; // y = 5, x = 7
```
执行赋值语句以后，返回被赋的value.

## Reference

* [Assignment Operator [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/operators/)

{% include links.html %}