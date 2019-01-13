---
title: "Arrange Coins to Make Stairs"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_arrange_coins_to_make_stairs.html
folder: algorithm
toc: false
---

## Description
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.
Given n, find the total number of full staircase rows that can be formed.
n is a non-negative integer and fits within the range of a 32-bit signed integer.

### Example
* Input: n = 5
  * Output: 2. Because the 3rd row is incomplete:
    ```
    #
    # #
    # #
    ```

## Solution：速度 前10%
利用 等差数列的求和公式：1 + 2 + 3 + ... + m = m * (m + 1) / 2

### Complexity
* Time: O(1)
* Space: O(1)

### Java
```java
public class Solution {
    public int arrangeCoins(long n) {
        if (n <= 0) {
            return 0;
        }
        
        int sqrt = (int)(Math.sqrt(n * 2));
        
        // 注意，这里做乘法要暂时把两个乘数换成long，因为这样才能保证 不会丢位数
        // 因为两个int相乘，乘积可能会突破int的范围，因为题意中说 n最大会到32bit
        if ((long)maxSqrtRoot * (long)(maxSqrtRoot + 1) <= n * 2) {
            return maxSqrtRoot;
        }
        return maxSqrtRoot - 1;
    }
}
```

## Reference
这题太简单了，没找网上出处了

{% include links.html %}
