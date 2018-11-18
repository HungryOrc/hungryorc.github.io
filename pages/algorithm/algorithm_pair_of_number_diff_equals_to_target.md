---
title: "Find a Pair of Numbers in the Array, whose Difference Equals to the Target"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_pair_of_number_diff_equals_to_target.html
folder: algorithm
toc: false
---

## Description
Given a sorted array A, find a pair (i, j) such that A[j] - A[i] is identical to a target number(i != j).

* If there does not exist such pair, return a zero length array.
* The given array is not null and has length of at least 2.
* 如果有多个pair符合条件，也只返回一个pair就行

### Example
* Input: A = {1, 2, 3, 6, 9}, target = 2
  * Output: return {0, 2} since A[2] - A[0] == 2
* Input: A = {1, 2, 3, 6, 9}, target = -2
  * Output: return {2, 0} since A[0] - A[2] == -2

## Solution
* 下面的hashmap的做法，适用于原数组sorted or unsorted。
  * 对于sorted array，这个做法可以做细微改进，以稍微再快一点：看array的排序是从大到小还是从小到大，看target是大于还是小于0，然后就只用判断 `cur-target` 或者 `cur + target` 二者中的一个就行，不必像下面的代码一样两方面都判断
* 这题只要求返回一个pair。如果需要返回所有pair，做法也是同理的

### Complexity
* Time: O(n)
* Space: O(n)，map

### Java
```java
public class Solution {
  public int[] twoDiff(int[] array, int target) {
    int n = array.length;
    
    // <value of element, index of element>
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < n; i++) {
      int cur = array[i];
      if (map.containsKey(cur - target)) {
        return new int[]{map.get(cur - target), i};
      }
      if (map.containsKey(cur + target)) {
        return new int[]{i, map.get(cur + target)};
      }
      map.put(cur, i);
    }
    
    return new int[0];
  }
}
```

## Reference
* [2 Difference In Sorted Array [LaiCode]](https://app.laicode.io/app/problem/278)

{% include links.html %}
