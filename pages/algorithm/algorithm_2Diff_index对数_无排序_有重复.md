---
title: "2 Diff, index对数，无排序，有重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Diff_index对数_无排序_有重复.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in an array that their difference equals to the given **positive** target number. 
Return the number of pairs of indices.
The given array is not null and has length of at least 2.

* 数组里没有重复的元素
* 目标差值target > 0，所以不会出现一个数减去自己等于target(即0)然后满足题意的情况，所以**我们就不必追究在代码运行的过程里，slow和fast这两个指针重合的时候怎么办**

### Example
* Input: A = {1, 3, 3, 1}, target = 2
  * Output: 4，因为有4对的差是2，它们的index pair分别是：[0, 1], [0, 2], [3, 1], [3, 2]. 谁见谁的前后关系无所谓

## Solution 1：先排序，再按排序后的array查2Diff的思路来做。慢

### Complexity
* Time: O(nlogn)，排序耗时
* Space: O(1)

## Solution 2：双Map！一个记录 cur - target在每个数的前面(左边)出现过几次，一个记录 cur + target在左边出现过几次

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int twoDiff(int[] nums, int target) {
        // ignore sanity checks
        
        int n = nums.length;
        int count = 0;
        
        // <value, number of occurance in the array of this value>
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int num : nums) {
            count += map.getOrDefault(num + target, 0);
            count += map.getOrDefault(num - target, 0);
            
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        return count;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
