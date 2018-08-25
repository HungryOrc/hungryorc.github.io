---
title: "Merge Sort"
tags: [algorithm, algorithm_overview, sorting]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_merge_sort.html
folder: algorithm
toc: false
---

## Overview

Merge Sort 可以用 Recursive 或 Iterative 的方式实现

## Implementation

### Java
```java
public class MergeSort {
  
    public int[] mergeSortOfIntArray(int[] array) {
        if (array == null || array.length <= 1) {
            return array;
        }
    
        // 方式1：Recursive merge sort
        int[] helperArray = new int[array.length];
        recurMergeSort(array, helperArray, 0, array.length - 1);
    
        // 方式2：Iterative merge sort
        // iterMergeSort(array);
    
        return array;
    }
  
    // ---------------------------------------------------------------------------------------
    // 以下的2个函数二选一使用，一个recursive，一个iterative
    // 对于merge sort来说，recursive way的代码更简洁，思路更漂亮，宜用resursive way
  
    // 方式1：Recursive
    private void recurMergeSort(int[] array, int[] helperArray, int start, int end) {
        if (start >= end) { // when there is only 1 element or 0 element
            return;
        }
    
        int mid = start + (end - start) / 2;
        recurMergeSort(array, helperArray, start, mid);
        recurMergeSort(array, helperArray, mid + 1, end);
    
        merge(array, helperArray, start, mid, end);
    }

    // 方式2：Iterative
    private void iterMergeSort(int array[]) {
        int n = array.length;
        int curSize;  // 当前每一段要被merge的 sub array 的大小：1,2,4,8......
        int leftStart;
        for (curSize = 1; curSize <= n-1; curSize = 2 * curSize) {
            // Pick starting point of different subarrays of current size
            for (leftStart = 0; leftStart < n - 1; leftStart += 2 * curSize) {
                // find ending point of left subarray. mid+1 is starting point of right subarray
                int mid = leftStart + curSize - 1;
                // find ending point of right subarray
                int rightEnd = Math.min(leftStart + 2 * curSize - 1, n - 1);
                // Merge two subarrays: array[leftStart...mid] and array[mid+1...rightEnd]
                int[] helperArray = new int[array.length];
                merge(array, helperArray, leftStart, mid, rightEnd);
            }
        }
    }
  
    // 以上的2个函数二选一使用
    // ---------------------------------------------------------------------------------------
 
    // 这一步是merge两个已经排好序的array。宗旨是：谁小移谁
    private void merge(int[] array, int[] helperArray, int start, int mid, int end) {
        if (start >= end) { // when there is only 1 element or 0 element
            return;
        }
    
        // copy the part between the start index and end index in the original array
        // to the helper array
        for (int i = start; i <= end; i++) {
            helperArray[i] = array[i];
        }
    
        // let the merge begin!
        int left = start;
        int right = mid + 1;
        int index = start;
        while (left <= mid && right <= end) {
            if (helperArray[left] <= helperArray[right]) {
                array[index] = helperArray[left];
                index ++;
                left ++;
            } else {
                array[index] = helperArray[right];
                index ++;
                right ++;
            }
        }
    
        // if there are still some elements left at the left side, we need to copy them
        while (left <= mid) {
            array[index] = helperArray[left];
            index ++;
            left ++;
        }
    
        // if there are still some elements left at the right side, we do nothing,
        // since they are already in their rightious position, hahahaha
    }
}
```

## Complexity
* Time: O(n*logn)
* Space: O(n)
  * 因为根据 Recursion Tree，每一层 call stack 都新建了 helper array，一共有 logn 层call stack，各个call stack所新建的 helper array的长度分别是 n/2, n/4, n/8... 2, 1。它们加在一起是 n 的长度。


{% include links.html %}
