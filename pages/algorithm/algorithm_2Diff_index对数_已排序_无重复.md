---
title: "2 Diff, index对数，已排序，无重复"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Diff_index对数_已排序_无重复.html
folder: algorithm
toc: false
---

## Description
Find all pairs of elements in a sorted array that their difference equals to the given target number. 
Return the number of pairs of indices.
The given array is not null and has length of at least 2.

* 数组里没有重复的元素

### Example
* Input: A = {1, 2, 3, 4}, target = 2
  * Output: 2，因为有两对的差是2：[1, 3]和[2, 4]

## Solution：快慢指针，都从左往右走，固定右边那个数，看左边有没有和它差为target的
我们可以肯定：右指针向右走的时候，左指针一定不会向左走，所以这就可以用sliding window了！

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int twoDiff(int[] nums, int target) {
        // ignore sanity checks
        
        int n = nums.length;
        int count = 0;
        int slow = 0, fast = 1;
        
        // 根据fast来做while loop比较好！因为可以及时发现右出界。根据slow来做while loop就不方便
        while (fast < n) {
            if (nums[slow] = nums[fast] - target) {
                fast++;
                slow++;
                count++;
            } else if (nums[slow] < nums[fast] - target) {
                slow++;
            } else {
                fast++;
            }
        }
        return count;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
