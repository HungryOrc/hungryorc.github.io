---
title: "Combination Sum，元素重复，不可复用"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_combinationSum_元素重复_不可复用.html
folder: algorithm
toc: false
---

## Description
* 数组里的元素可能有重复
* 在一个组合里，每个元素最多出现一次。比如数组里有三个1，那么每个1最多出现一次
* 在一个组合里，总的元素个数不限
* 在一个组合里，每个元素的出现顺序如果改变，还算是同一个组合

Given a collection of candidate numbers (C) and a target number (T), 
find all unique combinations in C where the candidate numbers sums to T.
Each number in C may only be used once in the combination.

Note:
* All numbers (including target) will be positive integers.
* Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
* The solution set must not contain duplicate combinations.

### Example
* Input: candidates = [10,1,6,7,2,1,5], target = 8,
  * Output: [[1,7], [1,2,5], [2,6], [1,1,6]]

## Solution: DFS

### Complexity
* Time: O(2^n) <=== 对么？？？
* Space: O(n)，每个list的可能size

### Java
```java
public class Solution {
    public List<List<Integer>> combinationSum2(int[] num, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if(num == null || num.length == 0) {
            return result;
        }
        
        Arrays.sort(num);
        
        dfs(num, 0, target, new ArrayList<Integer>(), result);
        return result;
    }
  
    private void dfs(int[] num, int startIndex, int remainTarget,
        ArrayList<Integer> combination, List<List<Integer>> result) {
        
        if (remainTarget == 0) {
            result.add(new ArrayList<Integer>(combination));
            return;
        }
        
        for (int i = startIndex; i < num.length; i++) {
            if (num[i] > remainTarget) {
                return;
            }
            
            // 精华所在！下面这个if语句是为了保证进入最终result的每个组合都是互不相同的！
            // 只有当数组里有重复元素时，才有这个if语句存在的必要！
            // 而这个if语句的存在，与数组里的每个元素能被使用一次还是无限次，无关！
            // 每个元素能被使用一次还是无限次，是靠下文的 startIndex 在下一轮dfs里
            // +1 还是 不+1 来区分实现的
            
            // 如果数组是 1, 2, 2, 4, 7, 9, 9, 9, 12
            //
            // 如果当前的 start index 在 1 那里，则在本次的 for 循环里，
            // 第一个2会入选，第二个2不会入选；第一个9会入选，第二三个9不会入选
            //
            // 如果当前的 start index 在 第一个2 那里，则在本次的 for 循环里，
            // 第一个2会入选，第二个2不会入选；第一个9会入选，第二三个9不会入选
            //
            // 如果当前的 start index 在第二个2 那里，则在本次的 for 循环里，
            // 第二个2会入选；第一个9会入选，第二三个9不会入选
            if (i > startIndex && num[i] == num[i - 1]) {
                continue;
            }
            
            combination.add(num[i]);
            // 一个元素最多只能用一次，所以下一次 recursion 的 start index 就必须 +1
            dfs(num, i + 1, remainTarget - num[i], combination, result);
            combination.remove(combination.size() - 1);
        }
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
