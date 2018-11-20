---
title: "Quick Select"
tags: [algorithm, algorithm_overview, sorting]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_quick_select.html
folder: algorithm
toc: false
---

## Overview
"Quick Select" is a selection algorithm **to find the kth smallest element in an unordered list**. 
It is related to the "Quick Sort" sorting algorithm.

注意
* Quick Select 完成以后，它左边的数都一定小于等于它，它右边的数都一定大于等于它。但是！**它左边的数并未被排序，它右边的数也并未被排序！**
* Quick Select 可以返回第k小的数的index或者value。如果要返回第k大，也是同理，**“第 k 大” 就是 “第 length-k+1 小”**
* 如果把较小的k个元素放到数组的左边，较大的 length-k 个元素放到数组的右边，**但是第k小的元素并不在分界点上，即并不在 index = k-1 的位置上，则这样做不算是 Quick Select**，因为并不知道第k小的数到底在哪里，是哪个，只知道左边的k个较小
  * 见下面的代码，有一种partition的处理是只管分割两端，并不保证具有分界值的元素最终处于分界点上

### Complexity
* Time: 
  * Best: O(n)
  * Average: O(n) <=== 怎么证明 ？？？
  * Worst: O(n^2)，当每次pivot都选到min或max
* Space: O(1) <=== 对么 ？？？
  
## Implementation 1，经典版

```java
public class Solution {
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
    
    // 和一般的 quick sort 的 partition函数 一样
    // 这个函数一方面会把小于pivot的数都放到左半边，另一方面会把pivot的index返回来
    // 在下面的 partition 的实现里，pivot的选取是选数组里的最右边的数。可以用别的选法，比如随机
    private int partition(int[] nums, int start, int end) {
        int left = start;
        int right = end - 1; // 别忘了 -1，因为 pivot取为最后一个数了，所以right只能取为倒数第二个
        int pivot  = nums[end];
        
        while (left <= right) {
            if (array[left] < pivot) {
                left ++;
            } else if (array[right] > pivot) {
                right --;
            } else {
                swap(array, left++, right--);
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

## Implementation 2，

```java



```

## Reference
网上没找到题目

{% include links.html %}
