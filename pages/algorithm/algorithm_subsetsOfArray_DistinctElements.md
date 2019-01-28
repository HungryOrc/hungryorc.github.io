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

## Solution 1: DFS, 用 Recursive Searching Tree 做
思路：看下一个位置可以放哪些数，而非某一个数是否出现在当前的subset里
```       
                   []
                /   |   \
            [1]    [2]    [3]
         /   \     /
    [1,2] [1,3]  [2,3]
     /
[1,2,3]
```

### Complexity
* Time: O(答案的个数 * 得到每个答案所需的时间) = O(2^n * n)
  * 得到每个答案所需的时间是 O(n)，是因为可能要把n个数都add到一个答案里去
* Space: O(n)，DFS的 call stack的层数 <=== 对么 ？？？？

### Java
代码挺短的
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
    
    // 这个函数是 把所有以curList 开头的组合 都放到result 里去
    private void dfs(int[] nums, int curIndex, List<Integer> curList, 
                     List<List<Integer>> result) {
        // 注意这题的一大特点！先什么都不管，先把curList扔到result里去！
        result.add(new ArrayList<Integer>(curList)); // 注意这里要new一个ArrayList
        
        // 下一个位置可以放哪些数
        for (int i = curIndex; i < nums.length; i++) {
            curList.add(nums[i]);
            dfs(nums, i + 1, curList, result); // 注意这里是 i+1，不要误用了 curIndex+1
            curList.remove(curList.size() - 1); // 复原
        }
    }
}
```

## Reference
* [Subsets [LeetCode]](https://leetcode.com/problems/subsets/description/)

{% include links.html %}
