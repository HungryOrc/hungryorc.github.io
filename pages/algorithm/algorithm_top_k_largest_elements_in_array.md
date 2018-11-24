---
title: "Top K Largest Elements in an Unsorted Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_top_k_largest_elements_in_array.html
folder: algorithm
toc: false
---

## Description
Given an unsorted integer array, find the top k largest numbers in it, and return them in a desending order in a new array.
There might be duplicate elements in the array.
You may assume that k is always valid, namely 1 <= k <= array's length.

Note that **it is the top k largest elements according to the sorted order, not the top k distinct and largest elements**.

这一题是找k个最大的数，不是找第k大的那个数

### Example
* Input: [3,10,1000,-99,4,100] and k = 3
  * Output: [1000, 100, 10]

## Solution 1: 用Max Heap装所有元素，比较符合直觉，但速度慢，不写了
步骤：
* Do max heapify on all the elements in the array, to form them into a max heap --> O(n) Time
* Call pop() on the max heap for k times, at the same time place them in a new array of size k, with the larger ones placed at the head of the new array --> O(klogn) Time

### Complexity
* Time: O(n + klogn)
* Space: O(n), size of the max heap <=== 对么 ？？？


## Solution 2: 用Min Heap装最大的k个元素。到时候就用这个方法
步骤：
* Do min heapify on the first k elements in the array, to form them into a min heap --> O(k) Time
* Iterate the rest n-k elements, for each of them, compare with the smallest one of the k elements in the min heap:
  * If new element <= top, do nothing
  * If new element > top, pop the top element, and add the new element into the min heap
  * At last, the top k largest elements of the array will be just the k elements in the min heap
  * Time of this step: O((n - k) * logk)
* Place the k elements in the min heap into a new array, and return that new array --> O(k) Time

### Complexity
* Time: O(k + (n - k) * logk)
* Space: O(k), size of the max heap <=== 对么 ？？？

### Java
```java
public class Solution {
    public int[] topk(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return nums;
        }
        
        int n = nums.length;
        
        // Java的PQ默认是 min heap，
        // 要造 max heap 的话：new PriorityQueue<>(10, Collections.reverseOrder())
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        for (int i = 0; i < k; i++) {
            minHeap.offer(nums[i]);
        }
        
        for (int i = k; i < n; i++) {
            if (nums[i] > minHeap.peek()) {
                minHeap.poll();
                minHeap.offer(nums[i]);
            }
        }
        
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            // 这样排在数组里将是从小到大的顺序
            result[i] = minHeap.poll();
        }
        return result;
    }
}
```

## Solution 3: 笨办法，Quick Sort 排序整个数组，再输出最大的k个，这样做是 over kill

### 这题不能用Quick Select来做！因为Quick Select只管两分边，但并不排序任何一边，而这题特别要求输出的结果要从大到小
算法笔记里专门有一篇讲 Quick Select，可以找那个看看。

### Complexity
* Time: O(nlogn)，用quick sort排序的时间
* Space: O(1) <=== 对么 ？？？

### Java
```java
public class Solution {
    public int[] topk(int[] nums, int k) {
        quickSort(nums, 0, nums.length - 1, k);

        int[] topk = new int[k];
        for (int i = 0; i < k; ++i)
            topk[i] = nums[i];

        return topk;
    }
    
    private void quickSort(int[] nums, int start, int end, int k) {
        if (start >= k)
            return;

        if (start >= end) {
            return;
        }
        
        int left = start, right = end;
        Random rand =new Random(end - start + 1);
        int index = rand.nextInt(end - start + 1) + start;
        int pivot = nums[index];

        // left的数大于pivot时left++，这样排出来是从大到小
        while (left <= right) {
            if (nums[left] > pivot) {
                left ++;
            } else if (nums[right] < pivot) {
                right --;
            } else {
                swap(nums, left++, right--);
            }
        }
        
        quickSort(nums, start, right, k);
        quickSort(nums, left, end, k);
    }
    
    private void swap(int[] nums, int i1, int i2) {
        int tmp = nums[i1];
        nums[i1] = nums[i2];
        nums[i2] = tmp;
    }
}
```

## Reference
* [Top k Largest Numbers [LintCode]](https://www.lintcode.com/problem/top-k-largest-numbers/description)

{% include links.html %}
