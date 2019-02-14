---
title: "Cut Wood: Cutting Cost Related to Length"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cutWood_cuttingCostRelatedToLength.html
folder: algorithm
toc: false
---

## Description
There is a wooden stick with length L >= 1, we need to cut it into pieces, where the cutting positions are defined in an int array A. 
The positions are guaranteed to be in ascending order in the range of [1, L - 1]. 
The cost of each cut is the length of the stick segment being cut. 
Determine the minimum total cost to cut the stick into the defined pieces.

在一个木头上的任何位置切一刀，这一刀的cost都和本段木头在 **切割前** 的长度成正比。
所以，按不同的顺序切开一个木头，最后的累计总cost是不同的。
然后这一题要求在所有标记的长度上最终都必须切开。

如果一个木头长为10，在2，4，7处都做了标记，那么就是要求必须在2米，4米，7米这三处都切开。
如果第一刀切2米处，则这一刀的cost为10；
然后第二刀切原先的4米处的话，这一刀的cost为8，因为10-2=8；
第三道切原先的7米处，这一刀的cost为6，因为切完第二刀之后，7米处所在的那一截的长度为6。
那么总cost就是 10+8+6=24。

### Example
* Input: L = 10, A = {2, 4, 7}
  * Output: the minimum total cost is 10 + 4 + 6 = 20 (cut at 4 first then cut at 2 and cut at 7)

## Solution：2维DP
思路：我们把题目给的数组做一个扩展，A = {2, 4, 7} --> {0, 2, 4, 7, 10}，即把开头和结尾的长度都包含进来了.

* DP数组 `M[i][j]` 表示 the min cost of cutting **EVERY POSITION** the wood between index i and index j in the input array A.
So for this example, the sulution we are looking for is `M[0][4]`

* Base Case: The adjacent cutting positions that cannot be cut any further: 
  * M[0][1] = 0, M[1][2] = 0, M[2][3] = 0... M[i-1][i] = 0

* Induction Rules
  * Size = 1: Adjacent indexes: [left = i, right = i + 1]
    M[0][1] = M[1][2] = M[2][3] = M[3][4] = 0



### Complexity
* Time: O(n^2)
* Space: O(n^2)

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
