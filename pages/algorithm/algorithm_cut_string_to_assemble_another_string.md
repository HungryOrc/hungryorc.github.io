---
title: "Cut a String in Every Way to Assemble Another String"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cut_string_to_assemble_another_string.html
folder: algorithm
toc: false
---

## Description
给了两个String A 和 B。尝试用 B 来组成 A。可以用多个B来组成A。组合之前可以把B的两端分别切掉0到任意个char(s)。
即B只能被削成一个更短的substring或者不削，而不能被切成两个或者更多个substring。
每个B被切的方式都可以不同或相同。
如果无论如何都无法做到组成A，则返回 -1。如果可以组成A，返回最少用多少个B（经历了上面描述的各种切之后的Bs）可以组成A。

### Example
* Input: A = "KAC", B = "JACK"
  * Output: 2。因为B即"JACK"可以用2个来组成A。第一个"JACK"被切为""

## Solution
我的解法。还需要

`A[0] < A[1] && A[A.length - 2] > A[A.length - 1]` 这个条件是非常关键的。它不仅显示了数组的第一个和最后一个元素都不可能是 local peak，
而且更重要的是揭示了如下规律：
* 如果某个元素a比它左边相邻的元素小，那么a与数组的开头之间（不包括a本身），一定存在至少一个peak
* 如果某个元素a比它左边相邻的元素大，那么a与数组的结尾之间（包括a本身），一定存在至少一个peak

用 iteration 的方式来做。

### Complexity
* Time: O(logn)
* Space: O(1)

### Java
```java
class Solution {

    public int findPeak(int[] A) {
        if (A == null || A.length < 3) {
            return Integer.MIN_VALUE;
        }
    
        // 题意说了，左右边界处不会是peak
        int left = 1, right = A.length-2; 
        
        while(start + 1 < end) {
            int mid = left + (right - left) / 2;
            
            // 题意说了，相邻元素之间不存在相等的情况
            if(A[mid] < A[mid - 1]) {
                right = mid - 1;
            } else { // A[mid] > A[mid - 1]
                left = mid;
            }
        }
        
        return (A[left] < A[right]) ? right : left;
    }
}
```

## Solution 2
思路和前面的Solution是一样的。只是用 recursion 的方式来做。

Binary Search 用 resursion 做是有点别扭，权当练习。

recursion做法竟然比 iteration做法快，beat LeetCode 99%，我感觉如果不用 call stack 应该会更快，为什么会反过来 ？？？

### Complexity
* Time: O(logn)
* Space: O(logn)，这是因为call stack 共有 logn 层，每层里面都是 O(1) 的空间消耗

### Java
```java
class Solution {

    public int findPeak(int[] nums) {
        if (nums == null || nums.length < 3) {
            return Integer.MIN_VALUE;
        }
        
        // 从第二个元素开始，到倒数第二个元素结束搜索。因为题目说了第一个和最后一个不是
        return findAnyPeak(nums, 1, nums.length - 2);
    }
    
    private int findAnyPeak(int[] nums, int left, int right) {
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[mid - 1]) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        
        return (nums[left] < nums[right]) ? right : left;
    }
}
```

## Reference
* 这是一道Laicode课上题目，尚未发现网上有此题

{% include links.html %}
