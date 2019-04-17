---
title: "Valid Parentheses in a Very Very Long String, by Multiple Machines"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validParenthesesString_三种括号_多台machine一起做.html
folder: algorithm
toc: false
---

## Description
这是Google的一个follow up题目。基本题在LC上有，名字是 `Valid Parentheses`。解出基本题以后，进一步的问题是：

一个超级超级长的String里有三种括号，没有别的种类的chars。用多个机器一起分析这个string，每个机器分析一段substring，最后要得到对于整个string的结论。
问如何实现。速度要越快越好。

### Example
无

## Solution
* 每个machine对于自己所负责的substring，先尽量地消除括号对，方法就是做基本题 `Valid Parentheses` 的那个方法。对于每个substring来说，可能能消干净，也很可能消不干净
* 每个substring消不干净留下来的部分，可能是下面几种中的一种（也许还有别的种类，我还没想到）：
  * `(((((((`
  * `)))))))`
  * `))))(((`，先左括号，再右括号
  * 括号一共有三种，上面的例子只举例了一种括号，但看上面的意思意会即可
* 对于最靠左的那个substring，出现遗留左括号是不行的。对于最靠右的那个substrin，出现遗留右括号也是不行的。中间的substring里遗留左右括号都行
* 从左往右考察每个substring遗留下来的左右括号。如果最后都归零，那就返回true。否则返回false
* 一个substring所含有的多余的左/右括号，并不需要一定要在它相邻右边的那个substring来抵消，完全可以是多个substing以后才抵消

### Complexity
略

### Java
```java
无
```

## Reference
网上没看见这题

{% include links.html %}
