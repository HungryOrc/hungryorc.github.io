---
title: "2 Sum, Number of Pairs of Indexes，已排序，有重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_index对数_已排序_有重复.html
folder: algorithm
toc: false
---

## Description
Find the number of pairs of elements in a sorted array that sum to the given target number. Return all the pairs of indices.
The given array is not null and has length of at least 2.

* 数组里有重复的元素，**返回的是 有多少对 合格的的indexes，所以重复元素但是index不同也算是不同的对象**
* 同一个元素（即同一个index）最多只能使用一次

### Example
* Input: A = {1, 1, 2, 2, 2, 3, 4}, target = 4
  * Output: 7，2个1和2个3能组成4对，3个2能组成3对，所以一共7对

## Solution
双指针，一前一后

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int allPairs(int[] nums, int target) {
        // ignore sanity checks
        
        int n = nums.length;
        int left = 0, right = n - 1;
        int count = 0;
        
        while (left < right) {
            int sum = nums[left] + nums[right];
            
            if (sum == target) {
            
                // 如果当前的数正好是target的一半
                if (nums[left] == nums[right]) {
                    count += (right - left + 1) * (right - left) / 2; // 巧妙！
                    break; // 可以彻底结束了
                }
            
                int rightCount = 1, leftCount = 1;

                while (left + 1 < right && nums[left] == nums[left + 1]) {
                    leftCount++;
                    left++;
                }
                while (right > left + 1 && nums[right] == nums[right - 1]) {
                    rightCount++;
                    right--;
                }

                left++;
                right--;

                count += leftCount * rightCount; // 巧妙！
            }
            else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        return count;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
