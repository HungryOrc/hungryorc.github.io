---
title: "2 Sum, Return All Valid Pairs of Unique Values"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_allValidPairsOfUniqueValues.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a given array that sum to the pair the given target number. 
Return all the **distinct pairs of values**.

The given array is not null and has length of at least 2. **The order of the values in the pair does not matter**.

* 数组里可能有重复的元素
* 每个元素最多可以被使用1次。重复的元素也可以各自被最多使用1次
* 返回的是一对一对的不同的values，而非indices，所以要去重，比如 [2, 4]和[2, 4]是一回事，[2, 4]和[4, 2]也是一回事

### Example
* Input: A = {1,0,-1,0,-2,2}, target = 0
  * Output: [[0, 0], [-1, 1], [-2, 2]]
* Input: A = {2, 1, 3, 2, 4}, target = 6
  * Output: [[2, 4]]

## Solution
和只用找一个pair的方法一样，用hashset来做

### Complexity
* Time: O(n) <=== 对么 ？？？？
* Space: O(n)，size of the Set

### Java
```java
public class Solution {
    public List<List<Integer>> allPairs(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        
        if (nums == null || nums.length == 0) {
            return result;
        }

        Set<Integer> visited = new HashSet<>();
        boolean foundHalf = false;
        
        for (int num : nums) {
            if (!visited.contains(target - num)) {
                visited.add(num);
                continue;
            }
            
            if (num * 2 == target) { // 别忘了 正好一半 的情况
                if (foundHalf) {
                    continue;
                }
                foundHalf = true;
            }
            
            List<Integer> pair = new ArrayList<>();
            pair.add(target - num);
            pair.add(num);
            result.add(pair);
            
            visited.add(num);
        }
        return result;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
