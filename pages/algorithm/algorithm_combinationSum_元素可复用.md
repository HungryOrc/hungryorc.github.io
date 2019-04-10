---
title: "Combination Sum，元素可复用"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_combinationSum_元素可复用.html
folder: algorithm
toc: false
---

## Description
* 数组里的元素不重复（但就算有重复也无所谓，因为这一题允许元素复用）
* 在一个组合里，元素可以复用
* 在一个组合里，总的元素个数不限
* 在一个组合里，每个元素的出现顺序如果改变，还算是同一个组合

Given a set of candidate numbers (candidates) **without duplicates** and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:
* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

### Example
* Input: candidates = [2,3,6,7], target = 7,
  * Output: [[7], [2,2,3]]
* Input: candidates = [2,3,5], target = 8,
  * Output: [[2,2,2,2], [2,3,3], [3,5]]

## Solution: DFS，速度 前1%

### Complexity
* Time: O(2^n) <=== 对么？？？
* Space: O(n)，每个list的可能size

### Java
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (candidates == null || candidates.length == 0) {
            return result;
        }
        
        // 这题元素可以复用，所以排序不是非做不可。但做了能加速后面的dfs！因为排序以后，
        // dfs的时候，来了一个元素，如果比remain sum to meet 还大，则后面的都可放弃
        Arrays.sort(candidates);
        
        dfs(candidates, 0, target, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int[] candidates, int startIndex, int remainTarget,
        ArrayList<Integer> combination, List<List<Integer>> result) {
        
        if (remainTarget == 0) {
            result.add(new ArrayList<Integer>(combination)); // 要new一个
            return;
        }
        
        for (int i = startIndex; i < candidates.length; i++) {
            if (candidates[i] > remainTarget) {
                return;
            }
        
            combination.add(candidates[i]);
            // 一个元素要无限次复用，
            // 下一次 recursion 的 start index 就必须与这一次的相同
            dfs(candidates, i, remainTarget - candidates[i], combination, result);
            combination.remove(combination.size() - 1);
        }
    }
}
```

## Reference
* [Combination Sum [LeetCode]](https://leetcode.com/problems/combination-sum/description/)

{% include links.html %}
