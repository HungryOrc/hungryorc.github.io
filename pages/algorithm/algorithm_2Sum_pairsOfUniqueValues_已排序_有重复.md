---
title: "2 Sum, Return All Valid Pairs of Unique Values，已排序，有重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_pairsOfUniqueValues_已排序_有重复.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a sorted array that sum to the pair the given target number. 
Return all the **distinct pairs of values**.

The given array is not null and has length of at least 2. The order of the values in the pair does not matter.

* 数组里可能有重复的元素
* 每个元素最多可以被使用1次。重复的元素也可以各自被最多使用1次
* 返回的是一对一对的不同的values，而非indices，所以要去重，比如 [2, 4]和[2, 4]是一回事，[2, 4]和[4, 2]也是一回事

### Example
* Input: A = {1,1,3,3,5}, target = 4
  * Output: [[1, 3]]

## Solution
两个指针，向中间走

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public List<List<Integer>> allPairs(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length <= 1) {
            return result;
        }
        
        int n = nums.length;
        int left = 0, right = n - 1;
        
        while (left < right) {
            int sum = nums[left] + nums[right];
            
            if (sum == target) {
                List<Integer> pair = new ArrayList<>();
                pair.add(nums[left]);
                pair.add(nums[right]);
                result.add(pair);
                
                // 这题关键在这里！
                // 当sum不等于target的时候，就算left或者right有重复值，也不必连续跳！接下来一步步跳就行了
                // 而sum等于target的时候，必须在此直接连续把重复的都去掉！
                while (left < right && nums[left + 1] == nums[left]) {
                    left++;
                } 
                while (left < right && nums[right - 1] == nums[right]) {
                    right--;
                }
            } else if (sum > target) {
                right--;
            } else {
                left++;
            }
        }
        return result;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
