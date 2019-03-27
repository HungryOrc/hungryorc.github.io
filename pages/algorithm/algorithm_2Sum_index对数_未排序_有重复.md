---
title: "2 Sum, Number of Pairs of Indexes，未排序，有重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_index对数_未排序_有重复.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a given array that sum to the given target number. Return the number of pairs of indices.
The given array is not null and has length of at least 2.

* 数组里可能有重复的元素，**但返回的是 有多少对indexes，所以重复元素但是index不同也算是不同的对象**
* 同一个元素（即同一个index）最多只能使用一次

### Example
* Input: A = {1, 3, 1, 3}, target = 4
  * Output: 4

## Solution
和只用找一个pair的方法一样，用hashmap做

### Complexity
* Time: O(n)  <=== 对么 ？？？？
* Space: O(n)，size of the Map

### Java
```java
public class Solution {
    public List<List<Integer>> allPairs(int[] nums, int target) {
        // ignore sanity checks
        
        int n = nums.length;
        int count = 0;
        Map<Integer, Integer> occurence = new HashMap<>();
        
        for (int i = 0; i < n; i++) {
            int cur = nums[i];
            count += occurence.getOrDefault(target - cur, 0); // 关键
            
            occurence.put(cur, occurence.getOrDefault(cur, 0) + 1);
        }
        return count;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
