---
title: "House Robber II: Houses in a Circle"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_house_robber_cycle.html
folder: algorithm
toc: false
---

## Description
哦也

### Example
* Input: 
  * Output: 

## Solution
这题和房子呈一条线的那题的唯一区别的情况是：如果偷了第一个房子的前提下的最优解是必须也偷最后一个房子，那么就违规了。其他的所有情况都和一条线的房子的情况是一样的

那么我们可以把所有情况分为两大类：
* 第一类，最后一个房子就是不让偷，只能偷从第1个到第n-1个房子之间的任何房子
* 第二类，最后一个房子可以偷，但第一个房子不让偷。那么就是从第2个房子到第n个房子随便偷

我们能像上面这么分割的根本原因在于：所有的合理解，要么第1个房子不偷，要么第n个房子不偷，要么第1个和第n个都不偷。但绝对不能第1个和第n个都不偷。所以 
**第1个不偷 / 第n个不偷 这2种情况可以覆盖所有的情况**

注意，第1个不偷 和 第n个不偷 这两种方法最后得到的答案可能是一样的！即最后的最优答案可能是第1个和第n个都不偷的时候！

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
