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
    // 以下的2个函数二选一使用
  
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
        int curr_size;  // 当前每一段要被merge的 sub array的大小：1,2,4,8......
        int left_start; // For picking starting index of left subarray to be merged
        // Merge subarrays in bottom up manner.  First merge subarrays of
        // size 1 to create sorted subarrays of size 2, then merge subarrays
        // of size 2 to create sorted subarrays of size 4, and so on.
        for (curr_size = 1; curr_size <= n-1; curr_size = 2 * curr_size) {
       
         // Pick starting point of different subarrays of current size
         for (left_start = 0; left_start < n-1; left_start += 2 * curr_size) {
           
             // Find ending point of left subarray. mid+1 is starting point of right subarray
             int mid = left_start + curr_size - 1;
              int right_end = Math.min(left_start + 2*curr_size - 1, n-1);
              // Merge Subarrays arr[left_start...mid] & arr[mid+1...right_end]
             int[] helperArray = new int[array.length];
             merge(arr, helperArray, left_start, mid, right_end);
         }
     }
  }
  
  // 以上的2个函数二选一使用
  // ---------------------------------------------------------------------------------------
 
  private void merge(int[] array, int[] helperArray, int start, int mid, int end) {
    if (start >= end) { // when there is only 1 element or 0 element
      return;
    }
    
    // copy the section between start and end in the array to the helper array, 
    // then we will merge the 2 parts in the helper array back to the original array
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
    // since they are already in their rightious position, haha
  }
}
```

## Complexity
* Time: O(n*logn)
* Space: O(n)
  * 因为根据 Recursion Tree，每一层 call stack 都新建了 helper array，一共有 logn 层call stack，各个call stack所新建的 helper array的长度分别是 n/2, n/4, n/8... 2, 1。它们加在一起是 n 的长度。


{% include links.html %}
