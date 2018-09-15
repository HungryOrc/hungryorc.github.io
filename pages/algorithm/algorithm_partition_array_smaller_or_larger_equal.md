---
title: "Partition Array: Smaller / Larger or Equal"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_partition_array_smaller_or_larger_equal.html
folder: algorithm
toc: false
---

## Description
Given an array nums of integers and an int k, partition the array (i.e move the elements in "nums") such that:
* All elements < k are moved to the left
* All elements >= k are moved to the right
Return the partitioning index, i.e the first index i nums[i] >= k.

### Example
* Input: nums = [3,2,2,1], k=2
  * Output: 1. Since after the partition, the array will be: [1,2,2,3].
  
## Solution
双指针，头尾相向。

注意这题要返回的是partition以后，从左往右第一个 >= k 的数的 index。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {

    public int partitionArray(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int len = nums.length;
        int left = 0, right = len - 1;
        
        while (left <= right) {
            while (left <= right && nums[left] < k) {
                left ++;
            }
            while (left <= right && nums[right] >= k) {
                right --;
            }
            
            if (left <= right) {
                swap(nums, left ++, right --);
            }
        }
        
        // 最终left一定在right的右边一位，指向第一个 >= k 的数，
        // 如果全数组所有数都 < k，则 left 将指向 len + 1，这也符合题目要求
        return left;
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## Reference
* [Partition Array [LintCode]](https://lintcode.com/problem/partition-array/description)

{% include links.html %}
