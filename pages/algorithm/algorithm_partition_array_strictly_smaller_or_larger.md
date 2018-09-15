---
title: "Partition Array: Strictly Smaller or Larger"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_partition_array_strictly_smaller_or_larger.html
folder: algorithm
toc: false
---

## Description
Given an array, and a pivotIndex. Rearrange the array so that the numbers smaller than array[pivotIndex] are on the left side of array[pivotIndex] and numbers larger than array[pivotIndex] are on the right side of array[pivotIndex].

Assumption: The array is not null or empty. pivotIndex is within the boundary of the array.

### Example
* Input: array = {9,4,6,2,0,1,7}, pivotIndex = 2
  * Output: {1, 4, 0, 2, 6, 9, 7}
  
## Solution 1
双指针，头尾相向。这个做法的好处是 空间方面 in-place。

双指针如果要做一边小于一边大于等于，或者一边小于等于一边大于，就很简单。但如果要做一边严格小于，一边严格大于，就飞鞋周折。
因为当两个指针中的一个或者两个都指向等于pivot的元素时，下一步怎么移，就有好几种情况需要区分处理，不是简单地swap然后左移右移就行的。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {

    public int[] partition(int[] nums, int pivotIndex) {
        if(nums == null || nums.length <= 1){
            return nums;
        }
        
        int len = nums.length;
        int pivot = nums[pivotIndex];
        
        // before "left" are the numbers strictly < pivot
        // after "right" are the numbers strictly > pivot
        int left = 0, right = len - 1;
        int helperIndex = left;
        
        while (left <= right) {
        	
        	while (left <= right && nums[left] < pivot) {
        		left ++;
            } // after this, left points to a number >= pivot

            while (left <= right && nums[right] > pivot) {
            	right --;
            } // after this, right points to a number <= pivot

            // Then, case by case:
            // ---------------------------------------------------------------------
            // left > pivot, right <= pivot
            if (nums[left] > pivot) {
            	swap(nums, left, right);
            	right --;
            	// now left might be == pivot, so don't increment left yet
            } 
            // left == pivot, right < pivot
            else if (nums[right] < pivot) {
            	swap(nums, left, right);
            	left ++;
            	// now right == pivot, so don't decrease right yet
            } 
            // left == pivot, right == pivot, this is a little bit tougher
            else {
              // 下面这一步很关键！没有这一步的话，时间就成了 O(n^2) 了！就不是 O(n)了
              helperIndex = Math.max(left + 1, helperIndex);
            	while (helperIndex <= right) {
            		if (nums[helperIndex] > pivot && right >= 0) {
            			swap(nums, helperIndex, right);
            			right --;
            			continue;
            		} else if (nums[helperIndex] < pivot && left < len) {
            			swap(nums, left, helperIndex);
            			left ++;
            			continue;
            		} else { // if nums[helperIndex] still == pivot
            		    helperIndex ++;
            		}
            	}
            	
            	// the following situation means by now everything between left and right
            	// are all == pivot, including left and right themselves,
            	// so the partitioning is done
            	if (helperIndex >= right) {
            		return nums;
            	}
            }
        }
        return nums;
    }
    
    private void swap(int[] nums, int i, int j) {
    	int tmp = nums[i];
    	nums[i] = nums[j];
    	nums[j] = tmp;
    }
}
```


## Reference
* [Partition [LaiCode]](https://app.laicode.io/app/problem/549)

{% include links.html %}
