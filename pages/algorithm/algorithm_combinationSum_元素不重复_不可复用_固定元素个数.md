---
title: "Combination Sum，元素不重复，不可复用，固定元素个数"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_combinationSum_元素不重复_不可复用_固定元素个数.html
folder: algorithm
toc: false
---

## Description
* 数组里的元素没有重复。且这些元素只能是 1到9 之间的整数！
* 在一个组合里，每个元素最多出现一次
* 在一个组合里，总的元素个数限定为 k 个，不能多不能少！
* 在一个组合里，每个元素的出现顺序如果改变，还算是同一个组合

Find all possible combinations **of k numbers** that add up to a number n, 
given that **only numbers from 1 to 9 can be used** and each combination should be a unique set of numbers.

### Example
* Input: k = 3, target = 7
  * Output: [[1,2,4]]
* Input: k = 3, target = 9
  * Output: [[1,2,6], [1,3,5], [2,3,4]]

## Solution: DFS

### Complexity
* Time: O(2^n) <=== 对么？？？
* Space: O(n)，每个list的可能size

### Java
```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if(k <= 0 || target <= 0) {
            return result;
        }
        
        dfs(1, k, target, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int curNum, int remainCount, int remainSum, 
                     List<Integer> list, List<List<Integer>> result) {
        if (remainSum == 0) {
            // 如果不固定k个数，那么把下面这个if语句去掉就行了
            if (remainCount != 0) {
                return;
            }
                
            result.add(new ArrayList<Integer>(list));
            return;
        }
        
        if (remainCount == 0 || curNum > 9) {
            return;
        }
        
        for (int num = curNum; num <= 9; num++) {
            list.add(num);
            dfs(num + 1, remainCount - 1, remainSum - num, list, result);
            list.remove(list.size() - 1); // 复原
        }
    }
}
```

## Reference
* [Combination Sum III [LeetCode]](https://discuss.leetcode.com/topic/37962/fast-easy-java-code-with-explanation)

{% include links.html %}
