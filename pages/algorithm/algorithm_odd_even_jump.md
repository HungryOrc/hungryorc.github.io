---
title: "Odd Even Jump"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_odd_even_jump.html
folder: algorithm
toc: false
---

## Description: Hard 题
You are given an integer array A.  From some starting index, you can make a series of jumps.  The (1st, 3rd, 5th, ...) jumps in the series are called odd numbered jumps, and the (2nd, 4th, 6th, ...) jumps in the series are called even numbered jumps.

You may from index i jump forward to index j (with i < j) in the following way:

* During odd numbered jumps (ie. jumps 1, 3, 5, ...), you jump to the index j such that A[i] <= A[j] and A[j] is the smallest possible value.  If there are multiple such indexes j, you can only jump to the smallest such index j.
* During even numbered jumps (ie. jumps 2, 4, 6, ...), you jump to the index j such that A[i] >= A[j] and A[j] is the largest possible value.  If there are multiple such indexes j, you can only jump to the smallest such index j.
* It may be the case that for some index i, there are no legal jumps.
A starting index is good if, starting from that index, you can reach the end of the array (index A.length - 1) by jumping some number of times (possibly 0 or more than once.)

Return the number of good starting indexes.

Note:
* 1 <= A.length <= 20000
* 0 <= A[i] < 100000

### Example
* Input: [10,13,12,14,15]
  * Output: 2
    * From starting index i = 0, we can jump to i = 2 (since A[2] is the smallest among A[1], A[2], A[3], A[4] that is greater or equal to A[0]), then we can't jump any more.
    * From starting index i = 1 and i = 2, we can jump to i = 3, then we can't jump any more.
    * From starting index i = 3, we can jump to i = 4, so we've reached the end.
    * From starting index i = 4, we've reached the end already.
    * In total, there are 2 different starting indexes (i = 3, i = 4) where we can reach the end with some number of jumps.
* Input: [2,3,1,1,4]
  * Output: 3
    * From starting index i = 0, we make jumps to i = 1, i = 2, i = 3:
      * During our 1st jump (odd numbered), we first jump to i = 1 because A[1] is the smallest value in (A[1], A[2], A[3], A[4]) that is greater than or equal to A[0].
      * During our 2nd jump (even numbered), we jump from i = 1 to i = 2 because A[2] is the largest value in (A[2], A[3], A[4]) that is less than or equal to A[1].  A[3] is also the largest value, but 2 is a smaller index, so we can only jump to i = 2 and not i = 3.
      * During our 3rd jump (odd numbered), we jump from i = 2 to i = 3 because A[3] is the smallest value in (A[3], A[4]) that is greater than or equal to A[2].
      * We can't jump from i = 3 to i = 4, so the starting index i = 0 is not good.
    * In a similar manner, we can deduce that:
    * From starting index i = 1, we jump to i = 4, so we reach the end.
    * From starting index i = 2, we jump to i = 3, and then we can't jump anymore.
    * From starting index i = 3, we jump to i = 4, so we reach the end.
    * From starting index i = 4, we are already at the end.
    * In total, there are 3 different starting indexes (i = 1, i = 3, i = 4) where we can reach the end with some number of jumps.
* Input: [5,1,3,4,2】
  * Output: 3
    * We can reach the end from starting indexes 1, 2, and 4.

## Solution：一个 Tree Map，两个 dp 数组
最暴力的方法的话，n平方可以做到。所以我们如果搞个 nlogn 的方法就ok

### Complexity
* Time: O(nlogn) <==== 对么？？？？
* Space: O(n) <==== 对么？？？？

### Java
```java
class Solution {
    public int oddEvenJumps(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int n = nums.length;
        boolean[] odd = new boolean[n];
        boolean[] even = new boolean[n];
        odd[n - 1] = true;
        even[n - 1] = true;
        
        // <value in the array, index in the array>
        // 这样不设置任何comparator的话，tree map 就默认根据 key 来排序，从小到大
        TreeMap<Integer, Integer> treeMap = new TreeMap<>();
        treeMap.put(nums[n - 1], n - 1);
        
        // 这一题最关键的地方在此！因为是从右到左地往 tree map 里放，
        // 所以tree map 里的东西，一定是当前元素右边的东西！所以就不必费心去限定必须是当前元素的右边了！
        for (int i = n - 2; i >= 0; i--) {
            int curNum = nums[i];
            
            // floorKey(K key)
            // returns the greatest key less than or equal to the given key, or null if there is no such key
            Integer biggestSmallerValueToTheRight = treeMap.floorKey(curNum);
            if (biggestSmallerValueToTheRight != null) {
                even[i] = odd[treeMap.get(biggestSmallerValueToTheRight)]; 
            }
            
            // ceilingKey(K key)
            // returns the least key greater than or equal to the given key, or null if there is no such key
            Integer smallestBiggerValueToTheRight = treeMap.ceilingKey(curNum);
            if (smallestBiggerValueToTheRight != null) {
                odd[i] = even[treeMap.get(smallestBiggerValueToTheRight)];
            }
            
            treeMap.put(curNum, i); // 用curNum的最左边的出现位置i来重置curNum在tree map 里的entry
        }
        
        int count = 0;
        // 只考察odd array，因为对于数组里所有的位置来说，它们的第一步都必然是从ood步(即1)开始
        for (int i = 0; i < n; i++) {
            if (odd[i] == true) count++;
        }
        return count;
    }
}
```

## Reference
* [Odd Even Jump [LeetCode]](https://leetcode.com/problems/odd-even-jump/description/)

{% include links.html %}
