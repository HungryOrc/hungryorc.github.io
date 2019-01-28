---
title: "Subsets of an Array of Distinct Elements"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_subsetsOfArray_DistinctElements.html
folder: algorithm
toc: false
---

## Description
Given an array of **distinct** integers, return all possible distinct subsets.

Note: 
* The solution set must not contain duplicate subsets.

### Example
* Input: nums = [1,2,3]
  * Output: 
    ```
    [
      [3],
      [1],
      [2],
      [1,2,3],
      [1,3],
      [2,3],
      [1,2],
      []
    ]
    ```

## Solution
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return result;
        }
        
        dfs(nums, 0, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int[] nums, int curIndex, List<Integer> curList, 
                     List<List<Integer>> result) {
        // 注意这题的一大特点！先什么都不管，先把curList扔到result里去！
        result.add(new ArrayList<Integer>(curList));
        
        for (int i = curIndex; i < nums.length; i++) {
            curList.add(nums[i]);
            dfs(nums, i + 1, curList, result);
            curList.remove(curList.size() - 1);
        }
    }
}
```

## Reference
* [Subsets [LeetCode]](https://leetcode.com/problems/subsets/description/)

{% include links.html %}
