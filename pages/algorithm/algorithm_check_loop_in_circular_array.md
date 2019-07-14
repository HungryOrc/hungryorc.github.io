---
title: "Check if there is any Loop in a Circular Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_check_loop_in_circular_array.html
folder: algorithm
toc: false
---

## Description
You are given a circular array nums of positive and negative integers. If a number k at an index is positive, then move forward k steps. Conversely, if it's negative (-k), move backward k steps. Since the array is circular, you may assume that the last element's next element is the first element, and the first element's previous element is the last element.

Determine if there is a loop (or a cycle) in nums. A cycle must start and end at the same index and the cycle's length > 1. Furthermore, movements in a cycle must all follow a single direction. In other words, a cycle must not consist of both forward and backward movements.

Note:
* -1000 ≤ nums[i] ≤ 1000
* nums[i] ≠ 0
* 1 ≤ nums.length ≤ 5000

Could you solve it in **O(n) time and O(1) extra space**?

### Example
* Input: [2,-1,1,2,2]
  * Output: True. There is a cycle, from index 0 -> 2 -> 3 -> 0. The cycle's length is 3.
* Input: [-1,2]
  * Output: False. The movement from index 1 -> 2 -> 1 -> ... is not a cycle, because movement from index 1 -> 2 is a forward movement, but movement from index 2 -> 1 is a backward movement. All movements in a cycle must follow a single direction.

## Solution 1: 我自己的方法，快慢指针。速度看起来较慢。我也不知道算不算是 O(n) 的速度。但code实现看起来和排名第一的解法相同？？？？ 我回头再多看下 ？？？？

确定不行的出发点，标 0 以便去重。希望由此方法来达到 O(n) 的时间复杂度

### Complexity
* Time: O(n^2) ？？？ 还是 O(n) ？？？  <====
* Space: O(1)

### Java
```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return false;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) continue;
            
            // indexes of fast and slow pointer
            Integer slow = i;
            int direction = nums[i] > 0 ? 1 : -1;
            Integer fast = getNextIndex(nums, direction, i);
            
            while (slow != null && fast != null && slow != fast) {
                slow = getNextIndex(nums, direction, slow);
                fast = getNextIndex(nums, direction, 
                                    getNextIndex(nums, direction, fast));
            }
            
            if (slow == fast) return true; // 发现了loop
            
            nums[i] = 0;
        }
        return false;
    }
    
    private Integer getNextIndex(int[] nums, int direction, Integer index) {
        if (index == null) return null; // 这是为了fast pointer 一次跳两步 的情况
        
        if (nums[index] == 0) return null;
        
        Integer result = null;
        int n = nums.length;
        int cur = nums[index];
        
        if (direction == 1 && cur < 0) return null;
        if (direction == -1 && cur > 0) return null;
        
        int next = index + cur;
        
        // 这一步特别重要！别忘了 一次前进可能会导致多次的穿越整个数组！当那个元素特别大的时候
        next %= n;
        
        if (cur > 0) {
            if (next > n - 1) result = next - n;
            else result = next;
        } else { // cur < 0
            if (next < 0) result = n + next;
            else result = next;
        }
        
        if (nums[result] == 0) return null;
        
        // 如果出现自循环，即一个元素自己loop到自己，则按题意这种情况不算circular
        if (result == index) return null;
        
        return result;
    }
}
```

## Reference
* [Circular Array Loop [LeetCode]](https://leetcode.com/problems/circular-array-loop/description/)

{% include links.html %}
