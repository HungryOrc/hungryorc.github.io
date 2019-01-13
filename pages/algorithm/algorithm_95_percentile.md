---
title: "95 Percentile"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_95_percentile.html
folder: algorithm
toc: false
---

## Description
Given a list of integers representing the lengths of urls, find the 95 percentile of all lengths 
(95% of the urls have lengths <= returned length)

Assumptions:
* The maximum length of valid url is 4096
* The list is not null and is not empty and does not contain null

### Example
* Input: [1, 2, 3, ..., 95, 96, 97, 98, 99, 100]
  * Output: 95 percentile of all lengths is 95

## Solution：类似于 Bucket Sort 的思想

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public class Solution {
    public int percentile95(List<Integer> lengths) {
        int n = lengths.size();
        int[] counts = new int[4097]; // so the range of lengths would be: [0, 4096]
    
        for (int len : lengths) {
            counts[len] ++;
        }
    
        double bar = n * 0.05;
        int cumulative = 0;
    
        // 从大到小
        for (int i = 4096; i >= 0; i--) {
            cumulative += records[i];
            if (cumulative > bar) { // 看题目里的例子，1到100，返回95而非返回96. 所以这里要用 >，而非 >=
                return i;
            }
        }
   
        return -1; // 其实永远不会到这里
    } 
}
```

## Reference
网上没找到这题

{% include links.html %}
