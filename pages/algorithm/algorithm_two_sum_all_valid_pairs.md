---
title: "Two Sum, All Valid Pairs"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_two_sum_all_valid_pairs.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a given array that sum to the given target number. Return all the pairs of indices.
The given array is not null and has length of at least 2.

注意，数组里可能有重复的元素，返回的是一对一对的indexes，所以重复元素但是index不同也算是不同的对象！

### Example
* Input: A = {1, 3, 2, 4}, target = 5
  * Output: [[0, 3], [1, 2]]

## Solution
和只用找一个pair的方法一样，用hashset来做

### Complexity
* Time: O(n^2)    《====== 对么 ？？？？
* Space: O(n)，size of set

### Java
```java
public class Solution {
    public List<List<Integer>> allPairs(int[] nums, int target) {
        // ignore sanity checks
        
        List<List<Integer>> result = new ArrayList<>();
        Map<Integer, List<Integer>> indexes = new HashMap<>();
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            int cur = nums[i];
            
            // 到目前为止，已经发现过的，和当前数cur能配对的数的indexes，都拿出来放入答案
            List<Integer> indexesOFMatchedNum = indexes.get(target - cur);
            if (indexesOfMatechedNum != null) {
                for (int index : indexesOfMatechedNum) {
                    List<Integer> pair = new ArrayList<>();
                    pair.add(i);
                    pair.add(index);
                    result.add(pair);
                }
            }
            
            // 把当前的数cur的index放到cur的value对应的map的list里去，因为可能有重复元素
            List<Integer> sameValueIndexes = indexes.getOrDefault(cur, new ArrayList<Integer>());
            sameValueIndexes.add(i);
            indexes.put(cur, sameValueIndexes);    <===== 这一步是不是可以不用 ？？？？
        }
        
        return result;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}