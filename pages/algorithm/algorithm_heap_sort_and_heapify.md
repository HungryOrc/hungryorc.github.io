---
title: "Heap Sort"
tags: [algorithm, algorithm_overview]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_heap_sort_and_heapify.html
folder: algorithm
toc: false
---

## Overview
Heap Sort 是一种可以 in-place 的排序算法。它的时间复杂度是 确保 O(nlogn)，而非平均 O(nlogn)。它的空间复杂度最优可以到 O(1)

A very good video for Heap Sort: [Heap Sort [GeeksforGeeks]](https://www.youtube.com/watch?v=MtQL_ll5KhQ)

Heap Sort的步骤：
* 要sort的array本身已经是一个array，即它已经满足heap必须是complete tree的性质（即tree nodes都要优先铺满左边）。现在这个没有sort的array可以说是一个乱序的heap，我们要把这个array从小到大sort的话，要借助max heap。从大到小排序的话，用min heap
* 首先要把这个乱序heap变成一个max heap。具体来说，就是对数组里 index = n / 2 - 1（即heap里从下往上的倒数第二层）一直到 index = 0 的各个元素 进行 heapify。要建立min heap，就搞min heapify；要建立max heap，就搞max heapify
  * **Heapify procedure can be applied to a node only if its children nodes are already heapified.** That's why heapify must be performed in the bottom up order.
  * 每次max heapify 的时间复杂度是 O(logn)。**但是build整个heap的时间是O(n)，不是O(nlogn)。** nlogn也没有错，因为build一个heap就是做n次heapify，但**nlogn不够tight**。要证明build heap只需要O(n)的话，需要一些数学，见此文：[Time Complexity of Building a Heap [GeeksforGeeks]](https://www.geeksforgeeks.org/time-complexity-of-building-a-heap/)
* 搞完这些之后，max heap已经建好了。heap顶部的元素，即目前数组的第一个元素，就是整个数组的最大元素
* 把max heap顶部的元素swap到数组的尾部（index = n - 1），即heap的最下一层最靠右的元素。然后对新swap到顶部的元素进行max heapify，在0到n-2的index范围内。所以这个耗时是 O(logn)，因为heap的高度一定是logn，因为heap是铺满的
* 再把新的max元素swap到heap末端，再重复上面的步骤

## Implementation 1: Space O(logn)，下面有Space O(1) 的做法
Ref: https://www.geeksforgeeks.org/heap-sort/

### Complexity
* Time: O(nlogn)
  * 每次 heapify 的时间是 O(logn)
  * 把乱序数组build成一个max heap需要 O(n)
  * 对于 `int lastIndex = n - 1; lastIndex >= 1; lastIndex --`，每个lastIndex做一次heapify，共做 n 次，时间消耗 O(nlogn)
* Space: O(logn)，因为call stack的层数是height of tree，而heap的层数一定是logn，因为heap是铺满的

### Java
```java
public class Solution {
    public void heapSort(int nums[]) {
        if (nums == null || nums.length <= 1) {
            return;
        }
        
        int n = nums.length;
        
        // build a max heap out of the given unsorted array
        // 每一次heapify的时间是logn，但是整个build heap的时间不是nlogn！而是n！证明见上文
        for (int i = n / 2 - 1; i >= 0; i--) {
            maxHeapify(nums, i, n);
        }
        
        // one by one, extract the max number from the max heap
        // heap里还剩一个元素的时候就不用搞了，所以lastIndex止于>=1处就行，不过写0也不会错
        for (int lastIndex = n - 1; lastIndex >= 1; lastIndex--) {
            // move current top (max) element to the end
            swap(nums, 0, lastIndex);
            
            // call max heapify on the reduced heap
            maxHeapify(nums, 0, lastIndex);
        }
    }
    
    // Max Heapify
    // 在数组nums里（即heap里），对这一段subarray做 max heapify：
    // 起始index为start，它即是当前subtree的root，
    // 终止index为end，它也表征了当前subtree的size
    private void maxHeapify(int[] nums, int start, int end) {
        // initialize the index of the largest to be root index
        int largest = start;
        int leftChild = start * 2 + 1;
        int rightChild = start * 2 + 2;
        
        // 下面两个if语句，是为了在start，leftChild，rightChild这三个index里，
        // 找到num值最大的一个。
        // 如果leftChild的index等于end，那do nothing! 因为end处已经放了前一轮的max
        if (leftChild < end && nums[leftChild] > nums[largest]) {
            largest = leftChild;
        }
        // 如果rightChild的index等于end，那do nothing! 因为end处已经放了前一轮的max
        if (rightChild < end && nums[rightChild] > nums[largest]) {
            largest = rightChild;
        }
        
        // if now largest is not root, swap largest and root
        if (largest != start) {
            swap(nums, start, largest);
            
            // recursively max heapify the AFFECTED subtree，具体来说是 start后移，end不变
            maxHeapify(nums, largest, end);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## Implementation 2: Space O(1)
用左半边array来 maintain the max heap。过程用iteration，不用recursion，所以没有多层的call stack 的空间消耗

### Complexity
* Time: O(nlogn)，同上理
* Space: O(1)

### Java
```java
public void heapSort(int[] array) {
    if (array == null || array.length <= 1) {
        return;
    }
    
    int n = array.length;
    
    // step 1, make a max heap in place
    heapify(array);
    
    // step 2, poll one from top and push to the end
    for (int i = n - 1; i >= 0; i--) {
        // put the largest at the rigth end of the array
        swap(array, 0, i);
        
        percolateDown(array, 0, i);
    }
}

private void heapify(int[] array) {
    int n = array.length;
    for (int i = n / 2 - 1; i >= 0; i--) {
        percolateDown(array, i, n);
    }
}

// iterative! not recursive!
private void percolateDown(int[] array, int index, int len) {
    while (index <= len / 2 - 1) {
        int left = index * 2 + 1; // index of left child of cur index
        int right = index * 2 + 2; // index of right child of cur index
        int max = index;
        
        if (left < len && array[left] > array[max]) {
            max = left;
        }
        if (right < len && array[right] > array[max]) {
            max = right;
        }
        
        if (max == index) {
            break;
        }
        
        swap(array, index, max);
        index = max;
    }
}

private void swap(int[] nums, int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
```

## Reference
* [Heap Sort [LaiCode]](https://app.laicode.io/app/problem/328)
* [Heap Sort Tutorial [GeeksforGeeks]](https://www.geeksforgeeks.org/heap-sort/)
* [Heap Sort Video [GeeksforGeeks]](https://www.youtube.com/watch?v=MtQL_ll5KhQ)
* [Time Complexity of Building a Heap [GeeksforGeeks]](https://www.geeksforgeeks.org/time-complexity-of-building-a-heap/)

{% include links.html %}
