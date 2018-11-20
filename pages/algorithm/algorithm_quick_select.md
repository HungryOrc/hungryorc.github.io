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
"Quick Select" is a selection algorithm to find the kth smallest element in an unordered list. 
It is related to the "Quick Sort" sorting algorithm.

注意
* Quick Select 完成以后，它左边的数都一定小于等于它，它右边的数都一定大于等于它。但是！**它左边的数并未被排序，它右边的数也并未被排序！**

### Complexity
* Time: 
  * Best: O(n)
  * Average: O(n) <=== 怎么证明 ？？？
  * Worst: O(n^2)，当每次pivot都选到min或max
* Space: O(1) <=== 对么 ？？？
  
## Implementation 1，经典版

```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return Integer.MIN_VALUE;
        }
        
        // 注意！找 第k个最大，就是找 第length-k+1个最小 ！！
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

## Implementation 2，经典版

### 比上面的写法繁琐，最重要的是每次partition能把分界值放到分界点上！靠最后那一下swap！

这种写法的特点：
* 每次跑一遍 `private int partition(int[] array, int start, int end)` 以后：
  * 这个函数返回的int值正好就是分界点的坐标！而且分界点此时的值能保证就是分界值！分界值的意思是说，它左边的所有数都保证大于等于它！它右边的所有数都保证小于等于它！这是这个写法比上一个写法强力的地方。做到这一点，是靠最后的那个 `swap(array, left, end)` 来保证的！
  * 两种quick sort的写法，最后sort的结果是一样的。所以如果不是特别需要这一点，也不用非要用这种写法。
  
```java
public class Solution {
    public int[] quickSort(int[] array) {
        if (array == null || array.length <= 1) {
            return array;
        }
        
        quickSort(array, 0, array.length - 1);
        return array;
    }
    
    // Overload quickSort method
    public void quickSort(int[] array, int left, int right) {
        if (left >= right) {
            return;
        }
        
        // the following method complete 2 tasks:
        // 1. get and return the pivotIndex
        // 2. partition the array using this pivot
        int pivotIndex = partition(array, left, right);

        // the pivot is already at the position where it should be, so 
        // when we do the recursion on the two partitions, pivot should NOT be included in any of them
        quickSort(array, left, pivotIndex - 1);
        quickSort(array, pivotIndex + 1, right);
    }

    private int partition(int[] array, int start, int end) {
        int pivotIndex = findPivotIndex(start, end);
        int pivot = array[pivotIndex];
        
        // swap the pivot element to the rightmost position
        swap(array, pivotIndex, end);
        
        int left = start;
        int right = end - 1;
        
        while (left <= right) {
            if (array[left] < pivot) {
                left ++;
            } else if (array[right] > pivot) {
                right --;
            } else {
                swap(array, left++, right--);
            }
        }
        // 这个while loop结束时，一定是：rightIndex 在 leftIndex 的左边一位 ！
        
        <=== 是么 ？？？ 有没有可能他们两中间间隔一位 ？？？因为在left==right的时候还可能进行left++且right--的操作！！！
        
        // 特别注意下面的处理方式 ！！！
        // ------------------------------------------------------------------------------------------------
        // 如果一开始pivot被移动到最右边，那么现在pivot要和leftIndex换，
        // 因为此时leftIndex指向的数一定 >= pivot ！！！
        // 如果一开始pivot被移动到最左边，那么现在pivot要和rightIndex换，
        // 因为此时rightIndex指向的数一定 <= pivot ！！！
        swap(array, left, end);
        
        // 这么移动之后，pivot 一定就位于我们接下来要return 的index的位置上，
        // 即这个位置的数一定 == pivot，而非含糊地 >= pivot 或 <= pivot ！！！
        // 注意了 ！！！
        // 之所以这里非要等于不可，不能有一丝的含糊，是因为再往后的 quick sort recursion里，
        // 前半段的quick sort 将结束于 pivot index - 1，
        // 后半段的quick sort 将开始于 pivot index + 1，
        // 所以 pivot index 上必须必须是等于 pivot 值的 ！！！因为以后再也不会碰这个 index 上的数了 ！！！
        // 至于 pivot index 之前或者之后的数，目前来说，完全可能含有n个等于pivot的数，这些都无所谓，
        // 但是在pivot位置上必须就是pivot值！！！然后下一步才有继续的基础！！！
        
        // 上面如果是 swap 了 right 和 start，这里就要 return right ！！！
        // 上面如果是 swap 了 left 和 end，这里就要 return left ！！！
        return left;
        // ------------------------------------------------------------------------------------------------
    }
    
    // random in the range of [left, right], inclusive in both ends
    // Math.random() 得出的是：[0, 1)
    private int findPivotIndex(int left, int right) {
        return left + (int)(Math.random() * (right - left + 1));
    }

    private void swap(int[] array, int left, int right) {
        int temp = array[left];
        array[left] = array[right];
        array[right] = temp;
    }
}
```

## Reference
网上没找到题目

{% include links.html %}
