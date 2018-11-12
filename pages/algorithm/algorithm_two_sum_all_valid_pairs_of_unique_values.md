---
title: "Two Sum, All Valid Pairs of Unique Values, Can Use Any Number Repeatedly"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_two_sum_all_valid_pairs_of_unique_values.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a given array that sum to the pair the given target number. 
Return all the **distinct pairs of values**.

The given array is not null and has length of at least 2. The order of the values in the pair does not matter

* 数组里可能有重复的元素，返回的是一对一对的不同的values，而非indices，所以要去重
* 但是由于同一个元素（即同一个index）可以反复使用，所以比如和等于6的话，一个3用两次是可以的！

### Example
* Input: A = {2, 1, 3, 2, 4, 3, 4, 2}, target = 6
  * Output: [[2, 4], [3, 3]]

## Solution
和只用找一个pair的方法一样，用hashset来做

### Complexity
* Time: O(n)
* Space: O(n)，size of the Set

### Java
```java
public class Solution {
    public List<List<Integer>> allPairs(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Set<Integer> visited = new HashSet<>();
        
        for (int num : nums) {
            if (!visited.contains(num)) {
                // 第一种情况
                // 注意，这里不要用 num == target / 2，否则会出现 7 / 2 = 3 这样的错误情况
                if (num * 2 == target) {
                    List<Integer> pair = new ArrayList<>();
                    pair.add(num);
                    pair.add(num);
                    result.add(pair);
                } 
                
                // 第二种情况
                else if (visited.contains(target - num)) {
                    List<Integer> pair = new ArrayList<>();
                    pair.add(num);
                    pair.add(target - num);
                    result.add(pair);
                }
                
                // 最后，无论什么情况，都要把当前的num加入到set里去
                visited.add(num);
            }
        }
        return result;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
