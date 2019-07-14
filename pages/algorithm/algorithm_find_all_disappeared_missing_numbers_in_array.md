---
title: "Find All Numbers Disappeared in an Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_all_disappeared_missing_numbers_in_array.html
folder: algorithm
toc: false
---

## Description
Given an array of POSITIVE integers where 1 ≤ a[i] ≤ n (n = size of array), 
some elements appear **twice** and others appear **once**.

Find all the elements of [1, n] inclusive that do not appear in this array.

难点如下（如果没下面这个要求，则这题非常简单）：

Could you do it **without extra space** and in **O(n) runtime**? 
You may assume the returned list does not count as extra space.

### Example
* Input: [4,3,2,7,8,2,3,1]
  * Output: [5,6]

## Solution 1: array类问题的典型常用方法："如果是正数，就将它变为负数"，必须记住！
Ref: https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/discuss/92956/Java-accepted-simple-solution

我们到了 nums[i] 的时候，如果 nums[i] = m，然后 nums[m - 1] > 0，则让 nums[m - 1] 的数乘以 -1，即正数变负数。
这么一变符号，意义在于：我们知道 m 出现过在数组里了。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        if(nums == null || nums.length == 0) return new ArrayList<>();
        
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            int curValueAbs = Math.abs(nums[i]);
            
            if (nums[curValueAbs - 1] > 0 )  {
                nums[curValueAbs - 1] *= -1; 
            }
        }
        
        List<Integer> result = new ArrayList();
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) result.add(i + 1);
        }
        return result;
    }
}
```

## Solution 2: array类问题的典型常用方法：非常巧妙而重要的swap方法，再加上一个有趣的while loop，我还需要再继续多看几次！

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        if(nums == null || nums.length == 0) return new ArrayList<>();
        
        int n = nums.length;
        
        for(int i = 0; i < n; i++) {
            // 特别注意！下面这个while loop 是整个方法的灵魂所在
            // 这里不要写：while (nums[i] - 1 != i) {...}，因为这么写可能永远也出不去 while loop
            while(nums[i] != nums[nums[i] - 1]){
                swap(nums, i , nums[i] - 1);
            }
        }
        
        List<Integer> result = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            if(nums[i] != i + 1) result.add(i + 1);
        }
        return result;
    }
    
    private void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

## Reference
* [Find All Numbers Disappeared in an Array [LeetCode]](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/)

{% include links.html %}
