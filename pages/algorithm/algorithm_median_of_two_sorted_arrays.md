---
title: "Median of Two Sorted Arrays"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_median_of_two_sorted_arrays.html
folder: algorithm
toc: false
---

## Description: Hard，但其实不是那么难
Given two sorted arrays of integers in ascending order, find the median value.

Assumptions
* The two given array are not null and at least one of them is not empty
* The two given array are guaranteed to be sorted

### Example
* Input: A = {1, 4, 6}, B = {2, 3}
  * Output: median is 3
* Input: A = {1, 4}, B = {2, 3}
  * Output: median is 2.5

## Solution: 速度 前1%
用 find Kth Smallest Number 的思想。注意这里的k是第k个数，不是index等于k的数
* 如果两个数组的总长度 total length 是奇数，则 k = totalLen / 2 + 1；
* 如果是偶数，则median是最中间的两个数之和除以二，即 median = ( 第(totalLen/2)小的数 + 第(totalLen/2+1)小的数 ) / 2

为了达到Log的复杂度，我们可以：
每次在A，B取前k/2个元素。有以下这些情况：
1) 如果A里的元素不够k/2个，丢弃B里的前k/2个元素. 反之亦然
2) 如果 A[mid] <= B[mid], (mid = k/2 - 1），丢弃A的前k/2个元素。反之亦然
3) 一开始k=k，然后k=k/2，k=k/4... 到最后k=1的时候，那么2个数组里下一个最小的数就是我们要的数了
4) 如果两个数组里的任何一个被丢弃光了，则自然剩下的数都在另一个数组里找

### Complexity
* Time: O(log (n1 + n2))
* Space: O(n) <==== ？？？？

### Java
```java
public class Solution {
    public double median(int[] nums1, int[] nums2) {
        int totalLen = nums1.length + nums2.length;
    
        if (totalLen % 2 == 1) { // median's index: k = totalLen / 2 + 1
            return findKthSmallest(nums1, 0, nums2, 0, totalLen / 2 + 1);
        } else { // median's index k = (totalLen / 2 + totalLen / 2 + 1) / 2;
            return (findKthSmallest(nums1, 0, nums2, 0, totalLen / 2) + 
                    findKthSmallest(nums1, 0, nums2, 0, totalLen / 2 + 1)) 
                    / 2.0;
        }
    }
  
    private int findKthSmallest(int[] nums1, int startIndex1, int[] nums2, int startIndex2, int k) {
        // 一共有 4 种情况
        // Case 1：两个数组里，有一个正好走完最后一位，不多不少。
        // 这个case必须放最前面，因为它关系到index溢出的问题！
        if (startIndex1 == nums1.length) {
            return nums2[startIndex2 + k - 1];
        }
        if (startIndex2 == nums2.length) {
            return nums1[startIndex1 + k - 1];
        }
    
        // Case 2：k 降低到1了，即再往下一个数就是我们要的数了
        if (k == 1) {
            return Math.min(nums1[startIndex1], nums2[startIndex2]);
        }
    
        // Case 3：两个数组里，有任何一个的长度不到 k/2 了
        // 如果A里剩下的的元素不够k/2个，丢弃B里的前k/2个元素. 反之亦然
        // 两个数组里都不足 k/2 个数是不可能的
        if (startIndex1 + k/2 - 1 >= nums1.length) {
            return findKthSmallest(nums1, startIndex1, nums2, startIndex2 + k/2, k - k/2);
        }
        if (startIndex2 + k/2 - 1 >= nums2.length) {
            return findKthSmallest(nums1, startIndex1 + k/2, nums2, startIndex2, k - k/2);
        }
    
        // Case 4：两个数组的长度都大于等于 k/2
        // 如果 nums1[mid] <= nums2[mid], (mid = k/2 - 1），丢弃nums1的 前k/2个元素。反之亦然
        if (nums1[startIndex1 + k/2 - 1] >= nums2[startIndex2 + k/2 - 1]) {
            return findKthSmallest(nums1, startIndex1, nums2, startIndex2 + k/2, k - k/2);
        } else {
            return findKthSmallest(nums1, startIndex1 + k/2, nums2, startIndex2, k - k/2);
        }
    }
}
```

## Reference
* [Median of Two Sorted Arrays [LeetCode]](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

{% include links.html %}
