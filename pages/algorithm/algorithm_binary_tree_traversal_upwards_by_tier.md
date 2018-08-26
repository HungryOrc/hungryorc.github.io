---
title: "Binary Tree Traversal Upwards by Tier"
tags: [algorithm, binary_tree, binary_tree_traversal]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_tree_traversal_upwards_by_tier.html
folder: algorithm
toc: false
---

## Overview
逐步给出 binary tree 的各个 leaf node，从下到上，从左到右。要求按照 Tier 输出结果，而非按照 Level 输出结果！这两种方式的区别如下：
```
      A
    /   \
   B     C
       /   \
      D     E
     / \
    F   G
```
都是从下到上，从左到右的遍历：
* 如果按照 Level，则结果为：[[F,G], [D,E], [B,C], A]
* 如果按照 Tier，则结果为：[[B,F,G,E], D, C, A]
  * B,F,G,E 都是 leaf，它们属于同一个“阶层” (tier)。
  * 如果把上面这几个 leaves 都去掉，那么剩下的树，leaf tier 就是 D。C 和 D 不是一个阶层，C 是更高一个阶层的。去掉 D 以后，C 才变成 leaf。


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
