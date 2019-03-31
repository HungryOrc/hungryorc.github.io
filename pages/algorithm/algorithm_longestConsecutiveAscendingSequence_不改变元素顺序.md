---
title: "Longest Consecutive Ascending Sequence: 不能改变元素在原数组里的顺序"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longestConsecutiveAscendingSequence_不改变元素顺序.html
folder: algorithm
toc: false
---

## Description
Given an unsorted array of integers, find the length of the longest consecutive ascending sequence
* sequence 的意思是选取的元素的位置未必要紧贴在一起
* consecutive ascending 的意思是选取元素的值必须逐个之间只相差1，且单调上升

本题要求
* 对于sequence的寻找，不能改变元素在原数组里的顺序！比如给数组 {1,3,4,2}，那么答案是2，因为每个元素的位置都不能变，所以满足要求的 longest 
consecutive ascending sequence 只能是 {1,2} 或者 {3,4}
* Your algorithm should run in O(n) complexity. 这也是一个难点。否则本题不那么难写

### Example
* Input: {100,4,-1,0,200,1,3,2}
  * Output: 4，即 {-1,0,1,2}

## Solution：我自己的想法，从左到右遍历数组，用一个map，记录 <value, consecutive sequence length includineg itself>

### Complexity
* Time: O(n)
* Space: O(n), map size

### Java
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // <value, consecutive sequence length includineg itself>
        Map<Integer, Integer> map = new HashMap<>();
        int maxLen = 1;

        for (int num : nums) {
            Integer lenSelf = map.get(num);
            Integer lenMinusOne = map.get(num - 1);

            if (lenSelf == null && lenMinusOne == null) {
                map.put(num, 1);
            } else if (lenSelf == null) {
                lenSelf = lenMinusOne + 1;
                map.put(num, lenSelf);
                maxLen = Math.max(maxLen, lenSelf);
            } else if (lenMinusOne == null) {
                // do nothing
            } else { // 上面两个len都不为null
                lenSelf = Math.max(lenSelf, lenMinusOne + 1);
                map.put(num, lenSelf);
                maxLen = Math.max(maxLen, lenSelf);
            }
        }

        return maxLen;
    }
}
```

## Reference
* 网上没看见这题，leetcode上有和这题类似的题

{% include links.html %}
