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

## Solution 1，先排序数组，然后两边向中间逼近

### Complexity
* Time: O(n^2)  ????
* Space: O(n)  ????

### Java
```java
public class Solution {
    public int[] twoSum(int[] givenNumbers, int targetSum) {
        int[] output = new int[2];

        int indexLeft = 0;
        int indexRight = givenNumbers.length - 1;

        // copy the given array
        int[] givenNumbersCopy = new int[givenNumbers.length];
        for (int i = 0; i < givenNumbers.length; i++) {
            givenNumbers_Copy[i] = givenNumbers[i];
        }

        // "Dual-Pivot" Quick Sort
        // Sort the copy of the given array, from smaller to bigger
        Arrays.sort(givenNumbersCopy);

        while (indexLeft < indexRight) {
            if (givenNumbersCopy[indexLeft] + givenNumbersCopy[indexRight] == targetSum) {
                // find the indexes of these 2 numbers in the original given array
                for (int i = 0; i < givenNumbers.length; i ++) {
                    if (givenNumbers[i] == givenNumbersCopy[indexLeft])	{
                        output[0] = i;
                        break;
                    }
                }
                for (int i = givenNumbers.length - 1; i >= 0; i --) {
                    if (givenNumbers[i] == givenNumbersCopy[indexRight])	{
                        output[1] = i;
                        break;
                    }
                }
            } else if (givenNumbersCopy[indexLeft] + givenNumbersCopy[indexRight] < targetSum) {
                indexLeft++;
            } else
                indexRight--;
            }
        }
        return output;
    }
}
```

## Solution 2，把数组排序，然后一个 for loop 包一个2Sum，2Sum 用 HashSet 做

### Complexity
* Time: O(n^2)
* Space: O(n)，size of set

### Java
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[0];
        }
        
        int n = nums.length;
        
        // map<element value in the array, element index>
        // 因为题意说了有且只会有一组答案，所以map里的一个key不会有2个value
        Map<Integer, Integer> elements = new HashMap<>();
        
        for (int i = 0; i < n; i++) {
            int cur = nums[i];
            
            Integer indexOfMatchingNum = elements.get(target - cur);
            if (indexOfMatchingNum != null) {
                return new int[]{i, indexOfMatchingNum};
            }
            
            elements.put(cur, i);
        }
        
        // 我们不会到这里
        return null;
    }
}
```

## Reference
[3Sum [LeetCode]](https://leetcode.com/problems/3sum/description/)

{% include links.html %}
