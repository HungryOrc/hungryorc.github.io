---
title: "Tower of Hanoi"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_tower_of_hanoi.html
folder: algorithm
toc: false
---

## Description
You are given an integer array `nums` and you have to return a new `counts` array. 
In the `counts` array, `counts[i]` is the number of smaller elements to the right of `nums[i]`.

### Example
* Input: nums = [5, 2, 6, 1]
  * Output: counts = [2, 1, 1, 0]

## Solution 1: Merge Sort
Ref: https://leetcode.com/problems/count-of-smaller-numbers-after-self/discuss/76583/11ms-JAVA-solution-using-merge-sort-with-explanation

我们要求的是，对于数组里nums[i]，要找数组里index > i
我们要求的东西是，对于nums[i]，要找数组里index的
我们要求的东西是，对于nums[i]，要找数组里index数
我们要求的东西是，对于nums[i]，要找数组里index
如果我们sort数组里的numbers，那么它们之间的顺序就丢失了，就算知道数组里有几个数小于nums[i]，我们也不知道这些小于它的数，它们的indexes是否大于i。

### Complexity
* Time: O(n logn)
* Space: O(n)

### Java
```java
public class Solution {
  int[] count;
  public List<Integer> countSmaller(int[] nums) {
      List<Integer> res = new ArrayList<Integer>();     

      count = new int[nums.length];
      int[] indexes = new int[nums.length];
      for(int i = 0; i < nums.length; i++){
       indexes[i] = i;
      }
      mergesort(nums, indexes, 0, nums.length - 1);
      for(int i = 0; i < count.length; i++){
       res.add(count[i]);
      }
      return res;
  }
  private void mergesort(int[] nums, int[] indexes, int start, int end){
   if(end <= start){
    return;
   }
   int mid = (start + end) / 2;
   mergesort(nums, indexes, start, mid);
   mergesort(nums, indexes, mid + 1, end);

   merge(nums, indexes, start, end);
  }
  private void merge(int[] nums, int[] indexes, int start, int end){
   int mid = (start + end) / 2;
   int left_index = start;
   int right_index = mid+1;
   int rightcount = 0;    	
   int[] new_indexes = new int[end - start + 1];

   int sort_index = 0;
   while(left_index <= mid && right_index <= end){
    if(nums[indexes[right_index]] < nums[indexes[left_index]]){
     new_indexes[sort_index] = indexes[right_index];
     rightcount++;
     right_index++;
    }else{
     new_indexes[sort_index] = indexes[left_index];
     count[indexes[left_index]] += rightcount;
     left_index++;
    }
    sort_index++;
   }
   while(left_index <= mid){
    new_indexes[sort_index] = indexes[left_index];
    count[indexes[left_index]] += rightcount;
    left_index++;
    sort_index++;
   }
   while(right_index <= end){
    new_indexes[sort_index++] = indexes[right_index++];
   }
   for(int i = start; i <= end; i++){
    indexes[i] = new_indexes[i - start];
   }
  }
}
```


## Solution 2: Segment Tree

## Solution 3: 从尾部开始做 + ArrayList + 二分查找
Ref: https://discuss.leetcode.com/topic/31173/my-simple-ac-java-binary-search-code

对于数组里最右边的那个数，它右边比它小的数一定是0个，从它开始往左推导。Maintain an sorted ArrayList of numbers that had been visited. 

Use findIndex() to find the index of the 1st element in the sorted ArrayList which is larger or equal to the target number. 
For example, [5,2,3,6,1], by the time we reach 2, we should already have a sorted array: [1,3,6], 
findIndex() returns 1, which is the index where 2 should be inserted and is ALSO the number smaller than 2. 
Then we insert 2 into the sorted array to form [1,2,3,6], and set `counts[1]` to be 1 (relevant to the 2 at `nums[1]`).

Then keep doing this till the start of array `nums`.

### Complexity
* Time: O(n^2). 这个解法的思路是很巧妙，但是它的时间效率并没有比暴力方法高。所以这个方法并不好。
  * Every insertion into the Arraylist costs O(n) time, since it has to move all the subsequent elements one slot towards the end.
  * Use binary search in the ArrayList for each time cost only O(logn), but since insertion costs O(n), so finding and insertion together costs O(n).
* Space: O(n)

### Java
```java
public class Solution {
    
    public List<Integer> countSmaller(int[] nums) {
        Integer[] result = new Integer[nums.length];
        List<Integer> sortedAL = new ArrayList<>();
        
        // 从后往前！
        for (int i = nums.length - 1; i >= 0; i--) {
            int curNum = nums[i];
            int numOfSmallerNumsAfterSelf = getThe1stNumThatIsBiggerOrEqualToTargetInAL(curNum, sortedAL);
            
            // 前一个参数是插入的位置，后一个参数是插入的值
            sortedAL.add(numOfSmallerNumsAfterSelf, curNum);
            
            result[i] = numOfSmallerNumsAfterSelf;
        }
        return Arrays.asList(result); // 注意这个直接从数组转到ArrayList的函数！
    }
    
    // Binary Search
    // returns the index of the 1st number in the sorted arraylist that 
    // is bigger or equal to the target number
    private int getThe1stNumThatIsBiggerOrEqualToTargetInAL(int target, List<Integer> sortedAL) {
        if (sortedAL.size() == 0) {
            return 0;
        }
        
        int left = 0;     
        int right = sortedAL.size() - 1;
        
        // 特别注意！
        // 这里要特别处理一下后面的二分查找所无法解决的情况：就是整个查找区域里根本就没有我们要的案例.
        // 这一题我们要找第一个大于等于target的数，但也许整个sorted list里所有的数都小于target，
        // 如果是这样的话，如果还按照下面的二分的方式走，会return整个list的最末尾一位的index，
        // 但我们真正要的是插到list的最末尾一位的后面，而不是最末尾一位上，
        // 所以需要特别写一下下面这个 special case
        if (sortedAL.get(right) < target) {
            return right + 1;
        }
        
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            int num = sortedAL.get(mid);
            
            if (num >= target) {
                right = mid;
            } else { // <
                left = mid + 1;
            }
        }
        
        if (sortedAL.get(left) >= target) {
            return left;
        } else {
            return right;
        }
    }
}
```

## Reference
* [Count of Smaller Numbers After Self [LeetCode]](https://leetcode.com/problems/count-of-smaller-numbers-after-self/description/)

{% include links.html %}
