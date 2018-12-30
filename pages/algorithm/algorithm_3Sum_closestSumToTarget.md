---
title: "Three Sum, Return the Closest Sum to the Target Sum"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_3Sum_closestSumToTarget.html                               
folder: algorithm
toc: false
---

## Description
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

Note:
* The solution set must not contain duplicate triplets.
* [1,1,2] and [2,1,1] and [1,2,1] are considered as the same triplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: nums = [-1, 2, 1, -4], and target = 1
  * Output: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2)

## Solution，先排序数组，然后选第一个数，然后对于后两个数，两边向中间逼近

### Complexity
* Time: O(n^2)
* Space: O(1) <=== 对么 ？？？？

### Java
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        if (nums == null || nums.length < 3) {
            return Integer.MIN_VALUE;
        }
        
        Arrays.sort(nums);
        int closest = nums[0] + nums[1] + nums[2] - target;
        int n = nums.length;
        
        for (int i = 0; i < n - 2; i++) {
            int first = nums[i];
            int remain = target - first;
            int start = i + 1;
            int end = n - 1;

            while (start + 1 <= end) {
                int sum = nums[start] + nums[end];
                if (sum > remain) {
                    closest = updateClosest(first + sum, closest, target);
                    end--;
                } else if (sum < remain) {
                    closest = updateClosest(first + sum, closest, target);
                    start++;
                } else { // == remain
                    return target; // exact match
                }
            }
        }
        return closest;
    }
    
    private int updateClosest(int threeSum, int closest, int target) {
        if (Math.abs(threeSum - target) < Math.abs(closest - target)) {
            return threeSum;
        }
        return closest;
    }
}
```

## Reference
[3Sum Closest [LeetCode]](https://leetcode.com/problems/3sum-closest/description/)

{% include links.html %}
