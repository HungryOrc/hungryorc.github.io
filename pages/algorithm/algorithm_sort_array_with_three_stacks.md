---
title: "Sort Array with Three Stacks"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_sort_array_with_three_stacks.html
folder: algorithm
toc: false
---

## Description
略

### Example
略

## Solution
第一个stack放没排的，第二个stack放排好顺序的，第三个stack做中转

注意！用 `Deque<Integer> stack = new ArrayDeque<>()` 来实现 stack！

### Complexity
* Time: O(n^2)，相当于selection sort的时间复杂度
* Space: O(n)

### Java
```java
public class Solution {
    public void sortArrayWithThreeStacks(int[] nums) {
        if (nums == null || nums.length == 0) {
            return;
        }
    
        // stack 1, for unsorted
        Deque<Integer> unsorted = new ArrayDeque<>();
        for (int n : nums) {
            unsorted.push(n);
        }
        
        // stack 2, for sorted
        Deque<Integer> sorted = new ArrayDeque<>();
        
        // stack 3, as buffer
        Deque<Integer> buffer = new ArrayDeque<>();
        
        int len = nums.length;
        while (sorted.size() < len) {
            sorted.push(unsorted.pop()); // 每一轮，先放一个进来，作为起始
            
            while (!unsorted.isEmpty()) {
                int cur = unsorted.pop();
                // 把小的放到sorted stack里去，把大的放到 buffer stack 里去
                if (cur > sorted.peek()) {
                    buffer.push(sorted.pop());
                    sorted.push(cur);
                } else {
                    buffer.push(cur);
                }
            }
            
            dump(buffer, unsorted);
        }
        
        // 誊写排序后的结果
        for (int i = 0; i < len; i++) {
            nums[i] = sorted.pop();
        }
    }
    
    private void dump(Deque<Integer> from, Deque<Integer> to) {
        while (!from.isEmpty()) {
            to.push(from.pop());
        }
    }
}
```

## Reference
网上没有找到这题

{% include links.html %}
