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

## Solution 1: Quick Select，速度很快
算法笔记里专门有一篇讲 Quick Select，可以找那个看看。

### Complexity
* Time: 也就是用 Quick Select 方法找 第k小或第k大的数 的时间消耗。最优以及平均 O(n)，最差 O(n^2)
* Space: O(1) <=== 对么 ？？？

### Java
```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return Integer.MIN_VALUE;
        }
        
        // 找第k个最大，就是找第length-k+1个最小
        // quick select原本是用来找第k个最小，所以在这里转化一下
        return quickSelect(nums, 0, nums.length - 1, nums.length - k + 1);
    }
    
    // quick select 这个算法的定义就是 “找第 k 小的数”，k 也就是下面的最后一个参数
    private int quickSelect(int[] nums, int start, int end, int k) {
        int chosenIndex = partition(nums, start, end);
        
        if (chosenIndex < k - 1) { // 第 k 小的数的 index 是 k - 1
            return quickSelect(nums, chosenIndex + 1, end, k);
        } else if (chosenIndex > k - 1) {
            return quickSelect(nums, start, chosenIndex - 1, k);
        } else { // chosenIndex == k - 1
            return nums[chosenIndex];
        }
    }
    
    // 和一般的 quick sort 的 partition函数 一样。
    // 这个函数一方面会把小于pivot的数都放到左半边，
    // 另一方面，更重要的是，第k小的那个数最终会被会放在 index = k-1 的位置上！
    private int partition(int[] nums, int start, int end) {
        if (end <= start) {
            return start;
        }
        
        // pivot的选取是随机的，这样选的平均速度比其他选法（比如选最后一个元素）高很多！
        // 搞一个random的index，把pivot设为这个数，然后把pivot挪到数组的尾部
        int random = new Random().nextInt(end - start) + start;
        int pivot  = nums[random];
        swap(nums, end, random);
        
        int left = start;
        int right = end - 1; // 因为已把pivot放到尾部了，所以right只能取为倒数第二个
        
        while (left <= right) {
            if (nums[left] < pivot) {
                left ++;
            } else if (nums[right] > pivot) {
                right --;
            } else {
                swap(nums, left++, right--);
            }
        }
        
        // 最后left要么是紧贴着right并且在right的右边一位，要么是left和right重合
        swap(nums, left, end);
        return left;
    }
    
    private void swap(int[] nums, int i1, int i2) {
        int tmp = nums[i1];
        nums[i1] = nums[i2];
        nums[i2] = tmp;
    }
}
```

## Solution 2: 用Max Heap装所有元素，比较符合直觉，但速度慢，不写了
步骤：
* Do max heapify on all the elements in the array, to form them into a max heap --> O(n) Time
* Call pop() on the max heap for k times, at the same time place them in a new array of size k, with the larger ones placed at the head of the new array --> O(klogn) Time

### Complexity
* Time: O(n + klogn)
* Space: O(n), size of the max heap <=== 对么 ？？？


## Solution 3: 用Min Heap装最大的k个元素，速度比quick select慢一些
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

## Reference
* [Top k Largest Numbers [LintCode]](https://www.lintcode.com/problem/top-k-largest-numbers/description)

{% include links.html %}
