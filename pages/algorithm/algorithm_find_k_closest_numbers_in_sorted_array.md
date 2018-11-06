---
title: "Find K Closest Numbers (to a Target Value) in a Sorted Array"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_k_closest_numbers_in_sorted_array.html
folder: algorithm
toc: false
---

## Description
Given a sorted array, two integers k and x, find the k closest elements to x in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.
* The value k is positive and will always be smaller than the length of the sorted array.
* Length of the given array is positive and will not exceed 10^4
* Absolute value of elements in the array and x will not exceed 10^4

### Example
* Input: [1,2,3,4,5], k=4, x=3
  * Output: [1,2,3,4]
* Input: [1,2,3,4,5], k=3, x=-2
  * Output: [1,2,3]

## Solution
先找到最接近的那个数，然后以它为中心向两边扩展

### Complexity
* Time: O(log(n) + k)
* Space: O(1)

### Java
```java
class Solution {
    public List<Integer> findClosestElements(int[] nums, int k, int x) {
        // we already have the gurantee that k > 0 and k < nums.length
        if (nums == null || nums.length == 0) {
            return new ArrayList<Integer>();
        }
        
        // Step 1: 找到最接近的那一个数。用九章双星模板
        int n = nums.length;
        int iClosest = -1;
        int s = 0, e = n - 1;
        while (s + 1 < e) {
            int m = s + (e - s) / 2; // mid
            if (nums[m] > x) {
                e = m;
            } else if (nums[m] < x) {
                s = m;
            } else {
                iClosest = m;
                break;
            }
        }
        
        if (iClosest == -1) {
            if (Math.abs(nums[s] - x) > Math.abs(nums[e] - x)) {
                iClosest = e;
            } else { // 如果相等，也优先选左边那个，因为左边的更小
                iClosest = s;
            }
        }
        
        // Step 2: 在最接近的数附近再找 k-1 个次接近的数
        Deque<Integer> result = new LinkedList<>();
        result.add(nums[iClosest]);
        int numsFound = 1;
        int left = iClosest - 1, right = iClosest + 1;
        
        while (numsFound < k && left >= 0 && right < n) {
            int lNum = nums[left], rNum = nums[right];
            if (Math.abs(lNum - x) > Math.abs(rNum - x)) {
                result.addLast(rNum);
                right++;
            } else if (Math.abs(lNum - x) < Math.abs(rNum - x)) {
                result.addFirst(lNum);
                left--;    
            } else { // 二者与closest的举例相等，则先放小的那个，自然就是左边那个
                result.addFirst(lNum);
                left--;
            }
            numsFound++;
        }
        
        while (numsFound < k) {
            if (left >= 0) {
                result.addFirst(nums[left]);
                left--;
            } else if (right < n) {
                result.addLast(nums[right]);
                right++;
            }
            numsFound++;
        }
        
        return new ArrayList(result);
    }
}
```

## Reference
* [Find K Closest Elements [LeetCode]](https://leetcode.com/problems/find-k-closest-elements/description/)

{% include links.html %}
