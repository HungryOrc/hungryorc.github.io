---
title: "2 Sum, All Valid Pairs of Indexes，已排序，无重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_pairsOfIndices_已排序_无重复.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a sorted array that sum to the given target number. Return all the pairs of indices.
The given array is not null and has length of at least 2.

* 数组里没有重复的元素，**返回的是一对一对的indexes，所以重复元素但是index不同也算是不同的对象**
* 同一个元素（即同一个index）最多只能使用一次

### Example
* Input: A = {0, 1, 2, 3, 4}, target = 4
  * Output: [[0, 4], [1, 3]]

## Solution
双指针，一前一后

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public List<List<Integer>> allPairs(int[] nums, int target) {
        // ignore sanity checks
        
        List<List<Integer>> result = new ArrayList<>();
        int n = nums.length;
        int left = 0, right = n - 1;
        
        while (left < right) {
            int sum = nums[left] + nums[right];
            
            if (sum == target) {
                List<Integer> pair = new ArrayList<>();
                pair.add(nums[left]);
                pair.add(nums[right]);
                left++;
                right--;
            } else if (sum < target) {
                left++;
            } else { // sum > target
                right--;
            }
        }
        return result;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
