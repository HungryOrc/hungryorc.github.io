---
title: "Sort Array with Two Stacks"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_sort_array_with_two_stacks.html
folder: algorithm
toc: false
---

## Description
略

### Example
略

## Solution
第一个stack放没排的，第二个stack放排好顺序的（其左半段）以及作为暂存区（它的右半段）

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
        int numOfSorted = 0;
        
        int len = nums.length;
        while (sorted.size() < len) {
            sorted.push(unsorted.pop()); // 每一轮，先放一个进来，作为起始
            
            while (!unsorted.isEmpty()) {
                int cur = unsorted.pop();
                // 始终保持当前剩下的数里最大的那个在sorted stack的右半段的顶部
                if (cur < sorted.peek()) {
                    int curMin = sorted.pop();
                    sorted.push(cur);
                    sorted.push(curMin);
                } else {
                    sorted.push(cur);
                }
            }
            
            int curMin = sorted.pop(); // 先把这一轮最小的单拎出来
            
            // 把这一轮里经手的剩下的dump回unsorted stack里去
            numOfSorted++;
            dump(sorted, unsorted, len - numOfSorted);
            
            sorted.push(curMin); // 再把这一轮最小的放回sorted stack
        }
        
        // 誊写排序后的结果
        for (int i = 0; i < len; i++) {
            nums[i] = sorted.pop();
        }
    }
    
    private void dump(Deque<Integer> from, Deque<Integer> to, int dumpSize) {
        for (int i = 0; i < dumpSize; i++) {
            to.push(from.pop());
        }
    }
}
```

## Reference
网上没有找到这题

{% include links.html %}
