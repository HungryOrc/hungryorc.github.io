---
title: "Find the Smallest Number that is Larger than Target in a Sorted Array. Or Largest Smaller"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_smallest_number_larger_than_or_largest_number_smaller_than_target_in_sorted_array.html
folder: algorithm
toc: false
---

## Description
一个从小到大排好序的int数组。给一个 int target。返回数组里比target大的最小的数。如果不存在，则返回 Integer.MAX_VALUE。

如果要找比target小的最大的数，也是同理。在下面的代码里这两个都写了。可以对比。

### Example
略

## Solution
Binary Search, 九章style

注意，如果到了最后，left和right两个坐标上的数都小于等于target，则意味着整个数组上没有数大于target。反过来也是，如果最后2个坐标上的数都大于等于target，则意味着整个数组上没有数小于target。

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {
    public int findSmallestLarger(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return Integer.MAX_VALUE;
        }
        
        int n = nums.length;
        int left = 0, right = n - 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            // 因为要找最小的大于target的数，所以
            // mid等于target的时候，要把left设为mid，这是关键
            if (nums[mid] > target) {
                right = mid;
            } else { // nums[mid] <= target
                left = mid;
            }
        }
        
        // 要找小的那个，所以先判断left
        if (nums[left] > target) {
            return nums[left];
        } else if (nums[right] > target) {
            return nums[right];
        } else { // 别忘了数组里所有数都小于等于target的情况！
            return Integer.MAX_VALUE;
        }
    }

    public int findLargestSmaller(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return Integer.MIN_VALUE;
        }
        
        int n = nums.length;
        int left = 0, right = n - 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            // 因为要找最小的大于target的数，所以
            // mid等于target的时候，要把left设为mid，这是关键
            if (nums[mid] > target) {
                right = mid;
            } else { // nums[mid] <= target
                left = mid;
            }
        }
        
        // 要找小的那个，所以先判断left
        if (nums[left] > target) {
            return nums[left];
        } else if (nums[right] > target) {
            return nums[right];
        } else { // 别忘了数组里所有数都小于等于target的情况！
            return Integer.MAX_VALUE;
        }
    }
}
```

## Reference
* 网上没有找到这题

{% include links.html %}
