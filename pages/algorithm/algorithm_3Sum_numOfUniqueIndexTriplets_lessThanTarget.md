---
title: "3 Sum, Return All Unique Triplets of Indexes that Sum Less than Target"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_3Sum_numOfUniqueIndexTriplets_lessThanTarget.html                               
folder: algorithm
toc: false
---

## Description
注意这题 **要返回的是 unique triplets of indexes**，不是 triplets of element values。而 indexes 是永远不会重复的，所以 triplets of indexes 也永远不会重复，不管 element values 是否有重复。

Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.

Note:
* The solution set must not contain duplicate triplets of indexes.
* [1,1,2] and [2,1,1] and [1,2,1] (all the numbers here are indexes) are considered as the same triplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Follow up
Could you solve it in O(n^2) runtime?

### Example
* Input: nums = [-2,0,1,3], and target = 2
  * Output: 2
  * Because there are two triplets which sums are less than 2: [-2,0,1], [-2,0,3]

## Solution：双指针，挺巧妙的，具体逻辑见代码
参考的这个答案：https://leetcode.com/problems/3sum-smaller/discuss/68817/Simple-and-easy-understanding-O(n2)-JAVA-solution

### Complexity
* Time: O(n^2)，双层 loop
* Space: O(1)

### Java
```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        if (nums == null || nums.length < 3) {
            return 0;
        }
        
        Arrays.sort(nums);
        int n = nums.length;
        int count = 0;
        
        for (int i = 0; i < n - 2; i++) {
            int remain = target - nums[i];
            
            int left = i + 1, right = n - 1;
            while (left < right) {
                if (nums[left] + nums[right] < remain) {
                    count += right - left;
                    left++;
                } else {
                    right--;
                }
            }
        }
        return count;
    }
}
```

## Reference
[3Sum Smaller [LeetCode]](https://leetcode.com/problems/3sum-smaller/description/)

{% include links.html %}
