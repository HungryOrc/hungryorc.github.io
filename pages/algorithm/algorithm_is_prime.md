---
title: "If a Number is Prime Number or Not"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_is_prime.html
folder: algorithm
toc: false
---

## Description
看一个数是不是 prime number，即 质数。质数的意思是除了自己和1以外，没有别的因数

### Example
略

## Solution
先用 (int)Math.sqrt(num) 得到给定的int num 的平方根(double) 经过去尾法之后得到的 int，设为 sqrt，
然后从 int i = 2; i <= sqrt; i++，如果出现了因数，那么就判定为不是 prime。
注意这里 i 不要从 1 开始。

对于 num < 0，num == 0, 1 和 2 这几种特殊情况，要各自单独规定一下结果 是 true 还是 false

这个方法可能不是最快的方法，如果之后找到更好的解法，就更新在这里，并写上代码

### Complexity
* Time: O(sqrt(n))
* Space: O(1)

### Java
略
```java

```

## Reference
这题在网上没找到。但实在太简单了

{% include links.html %}
