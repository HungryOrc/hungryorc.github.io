---
title: "Move All Zeros Rightwards, Keeping Other Numbers' Order"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_move_zeros_rightwards_keep_order.html
folder: algorithm
toc: false
---

## Description
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

You must do this in-place without making a copy of the array.
Minimize the total number of operations.

### Example
* Input: [0, 1, 0, 3, 12]
  * Output: [1, 3, 12, 0, 0]
    
## Solution 1
两个指针，都往右走。我自己的方法。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
  
  public int[] moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
      return nums;
    }
    
    int len = nums.length;
    int nextZero = 0, nextNonZero = 0;
    
    while (nextZero < len && nextNonZero < len) {
      while (nextZero < len && nums[nextZero] != 0) {
        nextZero ++;
      }
      
      nextNonZero = nextZero; // 这一步很关键！没有这个就错了！
      while (nextNonZero < len && nums[nextNonZero] == 0) {
        nextNonZero ++;
      }
      
      if (nextZero < len && nextNonZero < len) {
        swap(nums, nextZero, nextNonZero);
        nextZero ++;
        nextNonZero ++;
      }
    }
    
    return nums;
  }
  
  private void swap(int[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
  }
}
```

## Solution 2
看破以后的做法：直接把不是0的都挪到左边去。然后后面都写0就行。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
  
  public int[] moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
      return nums;
    }
    
    int len = nums.length;
    int firstZero = 0;
    
    for (int i = 0; i < len; i++) {
      if (nums[i] != 0) {
        nums[firstZero] = nums[i];
        firstZero ++;
      }
    }
    
    for (int i = firstZero; i < len; i++) {
      nums[i] = 0;
    }
    
    return nums;
  }
}
```

## Reference

{% include links.html %}
