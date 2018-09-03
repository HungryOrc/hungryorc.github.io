---
title: "Find a Global Peak in an A-Shape Array with Duplicate Elements"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_global_peak_in_A-shape_array_with_duplicates.html
folder: algorithm
toc: false
---

## Description
一个整数数组 nums，有重复元素，整个数组的元素值先升后降，数组长度 >= 3。

要求找到全局最大值，返回其 index。

### 相似题目
数组先升后降，但是没有重复元素，找 global peak 那一题

### Example
* Input: [1, 2, 2, 2, 6, 4, 0, 0]
  * Output: 4 (index of 6)

## Solution
还是用二分法。但是如果一个元素的左边及右边相邻的元素都小于等于它，并不能判定这个元素就是 global peak。因为它们可能只是局部平台而已。
而整个数组里可能有0到任意个平台，这些平台有的也许就是全局peak，有的只是半山腰上的平台。

比如 `… 3, 2, 2, 1…` 那么第二个2可能被误判为全局峰值，因为它左边的2小于等于它，它右边的1也小于等于它。
但是显然在更广的范围里，2并不是全局peak。

所以，如果我们仍然采用以往典型的二分法，会有两种可能陷入的陷阱：
* 当前mid的左右相邻数都等于自己，那么下一步不知道该怎么修改left/right
* 当前mid的左右相邻数不都等于自己，但是它已经很靠近全局peak，甚至它自己其实就是全局peak了，
但是下一步的mid可能会踩在一个平台上，然后就把再下一步的搜索带错，从而错过真正的全局peak
* 当前left和right已经相邻或重合，然后它们都处于一个平台上，即它们的值相等，那么我们会判断这个平台就是全局peak，而事实上显然可能不是

要解决这些问题，我们必须：
* 每一步mid都和mid-1相比较，然后用mid和mid-1里更大的那个数来更新 当前已知的 全局peak
* 把mid和mid-1的关系分为三种，即一定要把二者相同的情况单列出来！如果二者 < 或 >，都可以用经典的二分法来处理下一步，
但如果二者相等，那么就要接着分别处理 `[left, mid-2]` 和 `[mid+1, right]` 这两个区间！这就是这题的做法与经典二分法的区别所在！

所以这是一种 Iteration 为表，Recursion 为里 的做法。详见下面代码。

### Complexity
* Time: O(n)是所有元素全部重复的情况，也是这题的 worst case。O(logn) 是如果没有或很少重复元素的情况。
* Space: O(logn) 是worst case 的 call stack 的空间占用 

### Java
```java
class Solution {

    public int findGlobalPeakInDuplicableArray(int[] nums) {
        if (nums == null || nums.length < 3) {
            return Integer.MIN_VALUE;
        }
        
        return findPeak(nums, 0, nums.length - 1);
    }
    
    private int findPeak(int[] nums, int left, int right) {
        if (left > right) {
            return Integer.MIN_VALUE;
        }
        
        int localPeak = nums[left];
        
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            
            //// 为什么要有下面这段？？？？？？
            if (mid == 0) {
                return Math.max(localPeak, nums[left], nums[right]);
            }
            ////////
            
            if (nums[mid] > nums[mid - 1]) {
                localPeak = Math.max(localPeak, nums[mid]);
                left = mid + 1;
            } else if (nums[mid] < nums[mid - 1]) {
                localPeak = Math.max(localPeak, nums[mid - 1]);
                right = mid - 2;
            } else { // nums[mid] == nums[mid - 1]
                localPeak = Math.max(localPeak, nums[mid]);
                
                // 如果遇到平台的情况，就回过头去！把从mid向外的两端 重新细耕一遍！
                int peakOfTwoSides = Math.max(
                    findPeak(nums, left, mid - 2), findPeak(mid + 1, right));
                return Math.max(localPeak, peakOfTwoSides);
            }
        }
        
        if (validIndex(left, nums.length)) {
            localPeak = Math.max(localPeak, nums[left]);
        }
        if (validIndex(right, nums.length)) {
            localPeak = Math.max(localPeak, nums[right]);
        }
        return localPeak;
    }
    
    private boolean validIndex(index, len) {
        return (index >= 0) && (index <= len - 1);
    }
}
```

## Reference

{% include links.html %}
