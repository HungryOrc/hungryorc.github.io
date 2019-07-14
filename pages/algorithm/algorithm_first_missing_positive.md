---
title: "First Missing Positive"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_first_missing_positive.html
folder: algorithm
toc: false
---

## Description: Hard 题，关键是时间空间要求都很严
Given an unsorted integer array, find the smallest missing positive integer.

**要求：Your algorithm should run in O(n) time and uses constant extra space.**

### Example
* Input: [3,4,-1,1]
  * Output: 2

* Input: [7,8,9,11,12]
  * Output: 1

* Input: [] or null
  * Output: 1

* Input: [2, 1, 3]
  * Output: 4

## Solution 1：我自己的 swap 方法，速度 前1%

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        if (nums == null || nums.length == 0) return 1; // 根据题目要求，返回 1
        
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            // 在这个数组里，我们想要的都是在这个范围里的正整数：[1, n]
            if (nums[i] <= 0 || nums[i] > n) continue;
            
            // match correctly
            if (i == nums[i] - 1) continue;
            
            // 这题的精华就在这个while loop 了！
            while (nums[i] >= 1 && nums[i] <= n && nums[i] != nums[nums[i] - 1]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return n + 1; // 根据题目要求，返回 1
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## Solution 2：也是一种swap的方法，速度前5%，很巧妙，参考链接如下
Ref: https://leetcode.com/problems/first-missing-positive/discuss/17083/O(1)-space-Java-Solution

The key here is to use swapping to keep constant space and also make use of the length of the array, which means there can be at most n positive integers. So each time we encounter an valid integer, find its correct position and swap. Otherwise we continue.

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int firstMissingPositive(int[] A) {
        int i = 0;
        while(i < A.length){
            if(A[i] == i+1 || A[i] <= 0 || A[i] > A.length) i++;
            else if(A[A[i]-1] != A[i]) swap(A, i, A[i]-1);
            else i++;
        }
        i = 0;
        while(i < A.length && A[i] == i+1) i++;
        return i+1;
    }
    
    private void swap(int[] A, int i, int j){
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```

## Solution 3: 我也想了很久这题能否用不swap而是标记各个元素的方法，但似乎不可能
因为这个数组里可能含有负数，可能含有0，任何介于int型的上下限之间的数都可能出现。

比如对于数组 [4, 999, -3, 1]，不swap而是标记的方法，需要我们在看到
第一位的那个4的时候，就把数组里的第4个数做一个特别的标记。注意这必须只是一种标记，而不可以是一种覆盖，就是说我们要保留第4位上的那个数的原值的某种
特征，以便于我们loop到第4位的时候，还能完整地提取它的原值的信息。比如上面这个数组里的第4个数是1，那么它说明这个数组里是存在1的，那么最小的缺失的
正整数只能是2或者大于2的了。

所以，如果原数组里保证每个元素都是正整数，那么我们可以把它标记为一个同绝对值的负数，这样既体现了“第4位所应该在的4是存在的”，又保证了我们真的loop到第4位的时候，第4位上真正居于此地的元素1也可以发挥它的作用，即体现出“第1位所应该在的1是存在的”。

但是，现在我们所能得到的原数组，里面的整数是任意的，所以我感觉无法标记。也许将来开智了以后能发现有办法。

## Reference
* [First Missing Positive [LeetCode]](https://leetcode.com/problems/first-missing-positive/description/)

{% include links.html %}
