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

注意！扩展的过程中并不用一个一个把元素加进结果里去，记录好左右端点的坐标，最后一次性加完就好

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
        int numsFound = 1;
        int left = iClosest - 1, right = iClosest + 1;
        while (numsFound < k) {
            // 下面这个if条件有两种“或”情况，都需要注意
            // 或的右边用 <= 而非 <，是因为二者与x的距离相等的话，要选小的那个，即左边那个
            if (right >= n || (left >= 0 && (x - nums[left]) <= (nums[right] - x))) {
                left--;
            } else{
                right++;
            }
            numsFound++;
        }
        
        System.out.println(left);
        
        List<Integer> result = new LinkedList<>();
        // 注意 i 的取值范围！left已经过左，必须+1；right已经过右，必须-1
        for (int i = left + 1; i < right; i++) {
            result.add(nums[i]);
        }
        return result;
    }
}
```

## Reference
* [Find K Closest Elements [LeetCode]](https://leetcode.com/problems/find-k-closest-elements/description/)

{% include links.html %}
