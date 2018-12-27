---
title: "Two Sum, there is Exactly One Valid Pair"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2sum.html                               
folder: algorithm
toc: false
---

## Description
Given an array of integers, return the **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would **have exactly one solution**, and you may not use the same element twice.

* 数组里可能有重复的元素，返回的是一对 indices，因为按照题意，有且只有一对符合要求的数
* 同一个元素（即同一个index）最多只能使用一次

### Follow Up
注意！这题要进行优化的话，要先问清楚，是时间上要优化，还是空间上要优化

如果采用先排序再处理的方法（下面写的第二个方法），那么使用哪种排序算法，就和优化的目的有关了。不是一定采用速度快的 quick sort 或者 merge sort

因为从时间消耗来说：
* quick sort是最优 nlogn
* merge sort是一定 nlogn
* selection sort是一定 n^2，最差

但如果是要优化空间效率，那么从空间消耗来说：
* merge sort是 n，因为它要搞新的数组出来放中间结果
* quick sort是O(height of tree)，比如O(logn)，因为它要call stacks，call stack的层数等于tree的高度，每一层消耗constant 的空间
* selection sort只要双层循环，没有recursion，空间消耗是O(1)，从空间的角度说，selection sort 反而是最省的

### Example
* Input: nums = [2, 7, 11, 15], target = 9
  * Output: [0, 1]
  * Because nums[0] + nums[1] = 2 + 7 = 9

## Solution 1，用 HashMap 做
前 1% 的速度

### Complexity
* Time: O(n)
* Space: O(n)，size of map

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

## Solution 2，先排序数组，然后两边向中间逼近
indexLeft, indexRight 分别指向数组第一个元素和最后一个元素，判断两个元素的和 targetSum 的大小关系
* 如果 A[indexLeft] + A[indexRight] == targetSum，那么找到两个下标返回即可
* 如果 A[indexLeft] + A[indexRight] < targetSum，说明两个数的和还不够大，把 indexLeft 右移
* 否则两个数和太大，把 indexRight 左移
* 直到两个 index 重合

### Complexity
* Time: O(n * logn)，其中排序用 O(n * logn)，两边向中间逼近用 O(n)
* Space: O(n)，size of map

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

## Reference
[Two Sum [LeetCode]](https://leetcode.com/problems/two-sum/description/)

{% include links.html %}
