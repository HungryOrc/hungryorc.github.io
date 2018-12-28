---
title: "Three Sum, Return All Unique Triplets (the Order of the 3 Numbers in a Triplet Doesn't Matter)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_3Sum_allUniqueTriplets.html                               
folder: algorithm
toc: false
---

## Description
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
* The solution set must not contain duplicate triplets.
* [1,1,2] and [2,1,1] and [1,2,1] are considered as the same triplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: nums = [-1, 0, 1, 2, -1, -4], target sum = 0, as this question said
  * Output: [[-1, 0, 1], [-1, -1, 2]]

## Solution 1，先排序数组，然后两边向中间逼近。速度较快

### Complexity
* Time: O(n^2)
* Space: O(1) <=== 对么 ？？？？

### Java
```java
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums); // O(nlogn) time
        List<List<Integer>> result = new ArrayList<>(); 
        
        // the 1st number in the triplet
        for (int i = 0; i < nums.length - 2; i++) {
            
            // dedupte the 1st number
            if (i == 0 || (i > 0 && nums[i] != nums[i - 1])) {
                int start = i + 1, end = nums.length - 1;
                int target = 0 - nums[i];
                
                while (start < end) {
                    if (nums[start] + nums[end] == target) {
                        result.add(Arrays.asList(nums[i], nums[start], nums[end]));
                        
                        while (start < end && nums[start] == nums[start + 1]) {
                            start++;
                        }
                        while (start < end && nums[end] == nums[end - 1]) {
                            end--;
                        }
                        
                        start++;
                        end--;
                    } else if (nums[start] + nums[end] < target) {
                        start++;
                    } else {
                        end--;
                    }
                }
            }
        }
        return result;
    }
}
```

## Solution 2，把数组排序，然后一个 for loop 包一个2Sum，2Sum 用 HashSet 做。实测速度非常慢

### Complexity
* Time: O(n^2)
* Space: O(n)，size of set

### Java
这个代码看起来不短，其实逻辑很简单
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length < 3) {
            return result;
        }
        
        int n = nums.length;
        Arrays.sort(nums); // O(nlogn) time
        
        // i indicates the 1st number in the triplet
        for (int i = 0; i < n - 2; i++) {
            
            // deduplicate the 1st number
            if (i >= 1 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            List<List<Integer>> pairs = twoSum(nums, i + 1, 0 - nums[i]);

            for (List<Integer> pair : pairs) {
                // in this way we are appending the 1st number last,
                // but doesn't matter, deduplication had been ensured beforehand
                pair.add(nums[i]);
                result.add(pair);
            }
        }
        return result;
    }
    
    // the input array is guaranteed to be sorted
    private List<List<Integer>> twoSum(int[] nums, int start, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        boolean foundTwoHalvesOfTarget = false;
        
        for (int i = start; i < nums.length; i++) {
            int cur = nums[i];
            
            if (set.contains(cur)) {
                if (cur * 2 == target && !foundTwoHalvesOfTarget) {
                    foundTwoHalvesOfTarget = true;
                    List<Integer> pair = new ArrayList<>();
                    pair.add(cur);
                    pair.add(cur);
                    result.add(pair);
                }
                continue;
            }

            if (set.contains(target - cur)) {
                List<Integer> pair = new ArrayList<>();
                pair.add(cur);
                pair.add(target - cur);
                result.add(pair);
            }
            set.add(cur);
        }
        return result;
    }
}
```

## Reference
[3Sum [LeetCode]](https://leetcode.com/problems/3sum/description/)

{% include links.html %}
