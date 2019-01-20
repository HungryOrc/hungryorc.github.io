---
title: "K Sum II, All Unique Solutions"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_k_sum_all_unique_solutions.html
folder: algorithm
toc: false
---

## Description
Given n **DISTINCT** integers (not necessary positive), an integer k (1 <= k <= n), 
and a number target (not necessary positive), 

Find all possible k-integers-combinations where their sum equals to target.

注意
* 数组里没有重复的数字！这对于解法特别重要，否则，解法就要有些改变之处

### Example
* Input: [1,2,3,4], k = 2, target = 5.
  * [[1, 4], [2, 3]]

## Solution: 朴素的 DFS，没什么花活儿，一直DFS到底，不借助 4sum或者2sum

### Complexity
* Time: O(n^k)，n 是数组的长度，k 是题目要求最终要取用多少个数 <=== 对么 ？？？？
* Space: O(n), DFS 里的 list 最多占用 n 的长度 <=== 对么 ？？？？

### Java
```java
public class Solution {
    public List<List<Integer>> kSumII(int[] nums, int k, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length == 0 || k <= 0) {
            return result;
        }
        
        dfs(nums, 0, k, target, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int[] nums, int curIndex, 
                     int remainElements, int remainSum,
                     List<Integer> curList, List<List<Integer>> result) {
        if (remainElements == 0 && remainSum == 0) {
            result.add(new ArrayList<Integer>(curList));
            return;
        }
        
        if (remainElements == 0 || curIndex == nums.length) {
            return;
        }
        
        // case 1: do not use the number at curIndex
        dfs(nums, curIndex + 1, remainElements, remainSum, curList, result);
        
        // case 2: use the number at curIndex
        curList.add(nums[curIndex]);
        dfs(nums, curIndex + 1, remainElements - 1, remainSum - nums[curIndex],
            curList, result);
        curList.remove(curList.size() - 1);
    }
}
```

## Reference
* [K Sum II [LintCode]](https://www.lintcode.com/problem/k-sum-ii/description)

{% include links.html %}
