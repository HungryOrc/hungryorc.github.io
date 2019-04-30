---
title: "Combination Sum，元素可复用，元素排序有关系 (更像是 Permutation Sum)"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_combinationSum_元素可复用_元素排序有关系.html
folder: algorithm
toc: false
---

## Description
* 数组里的元素不重复（但就算有重复也无所谓，因为这一题允许元素复用）
* 在一个组合里，元素可以复用
* 在一个组合里，总的元素个数不限
* 在一个组合里，每个元素的出现顺序如果改变，就视为新的组合！！

Given an integer array with all positive numbers and no duplicates, 
find the number of possible combinations that add up to a positive integer target.

### Example
* Input: candidates = [1, 2, 3], target = 4,
  * Output: all the possible combinations (actually permuations) are as follows:
  * 注意, 不同的顺序算是不同的组合！比如 (1, 1, 2) 和 (1, 2, 1) 算是2种组合
  ```
  (1, 1, 1, 1)
  (1, 1, 2)
  (1, 2, 1)
  (1, 3)
  (2, 1, 1)
  (2, 2)
  (3, 1)
  ```

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
* [Combination Sum IV [LeetCode]](https://leetcode.com/problems/combination-sum-iv/)

{% include links.html %}
