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
* Input: 
  * Output: 
Example 1:

Input: [1,2,0]
Output: 3
Example 2:

Input: [3,4,-1,1]
Output: 2
Example 3:

Input: [7,8,9,11,12]
Output: 1


## Solution：速度前5%，很巧妙，参考链接如下
Ref: https://leetcode.com/problems/first-missing-positive/discuss/17083/O(1)-space-Java-Solution

The key here is to use swapping to keep constant space and also make use of the length of the array, which means there can be at most n positive integers. So each time we encounter an valid integer, find its correct position and swap. Otherwise we continue.

### Complexity
* Time: O(n)
* Space: O(n)

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

## Reference
* [First Missing Positive [LeetCode]](https://leetcode.com/problems/first-missing-positive/description/)

{% include links.html %}
