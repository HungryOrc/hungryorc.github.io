---
title: "Two Sum, there is Exactly One Valid Pair"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_two_sum.html                               
folder: algorithm
toc: false
---

## Description
Given an array of integers, return the **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would **have exactly one solution**, and you may not use the same element twice.

* 数组里可能有重复的元素，返回的是一对 indices，因为按照题意，有且只有一对符合要求的数
* 同一个元素（即同一个index）最多只能使用一次

### Example
* Input: nums = [2, 7, 11, 15], target = 9
  * Output: [0, 1]
  * Because nums[0] + nums[1] = 2 + 7 = 9

## Solution
用hashset做。前 1% 的速度

### Complexity
* Time: O(n)
* Space: O(n)，size of map

### Java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[0];
        }
        
        int n = nums.length;
        
        // map<element value in the array, element index>
        // 因为题意说了有且只会有一组答案，所以map里的一个key不会有2个value
        Map<Integer, Integer> elements = new HashMap<>();
        
        for (int i = 0; i < n; i++) {
            int cur = nums[i];
            
            Integer indexOfMatchingNum = elements.get(target - cur);
            if (indexOfMatchingNum != null) {
                return new int[]{i, indexOfMatchingNum};
            }
            
            elements.put(cur, i);
        }
        
        // 我们不会到这里
        return null;
    }
}
```

## Reference
[Two Sum [LeetCode]](https://leetcode.com/problems/two-sum/description/)

{% include links.html %}
