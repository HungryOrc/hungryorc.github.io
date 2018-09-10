---
title: "Selection Sort"
tags: [algorithm, algorithm_overview, sorting]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_selection_sort.html
folder: algorithm
toc: false
---

## Overview
从小到大排序。从左到右遍历数组，找到最小的，然后把最小的和最左的swap。接下来从左边第二个开始重复上述过程。然后是左边第三个......

## Complexity
* Time: O(n^2)
* Space: O(1)

## Implementation

### Java
```java
public class SelectionSort {

    public int[] selectionSort(int[] nums) {
        if (nums == null || nums.length == 0) {
            return nums;
        }
        
        int len = nums.length;
        for (int i = 0; i < len - 1; i++) { // 不能终止于 i < len - 2，必须是 len - 1
            int indexOfMinInThisLoop = i; // 这一步是关键。同时注意这一步的摆放位置
            for (int j = i + 1; j < len; j++) {
                if (nums[j] < nums[indexOfMinInThisLoop]) {
                    indexOfMinInThisLoop = j;
                }
            }
            swap(nums, i, indexOfMinInThisLoop);
        }
        return nums;
    }
    
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

{% include links.html %}
