---
title: "RLE Iterator"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_RLE_iterator.html
folder: algorithm
toc: false
---

## Description
Write an iterator that iterates through a run-length encoded sequence.

The iterator is initialized by RLEIterator(int[] A), where A is a run-length encoding of some sequence.  More specifically, for all even i, A[i] tells us the number of times that the non-negative integer value A[i+1] is repeated in the sequence.

The iterator supports one function: next(int n), which exhausts the next n elements (n >= 1) and returns the last element exhausted in this way.  If there is no element left to exhaust, next returns -1 instead.

For example, we start with A = [3,8,0,9,2,5], which is a run-length encoding of the sequence [8,8,8,5,5].  This is because the sequence can be read as "three eights, zero nines, two fives".

Your RLEIterator object will be instantiated and called as such:
```java
RLEIterator obj = new RLEIterator(A);
int param_1 = obj.next(n);
```

Note:
* 0 <= A.length <= 1000
* A.length is an even integer.
* 0 <= A[i] <= 10^9
* There are at most 1000 calls to RLEIterator.next(int n) per test case.
* Each call to RLEIterator.next(int n) will have 1 <= n <= 10^9.

### Example
```
Input: ["RLEIterator","next","next","next","next"], [[[3,8,0,9,2,5]],[2],[1],[1],[2]]
Output: [null,8,8,5,-1]

Explanation: 
RLEIterator is initialized with RLEIterator([3,8,0,9,2,5]).
This maps to the sequence [8,8,8,5,5].
RLEIterator.next is then called 4 times:

.next(2) exhausts 2 terms of the sequence, returning 8.  The remaining sequence is now [8, 5, 5].

.next(1) exhausts 1 term of the sequence, returning 8.  The remaining sequence is now [5, 5].

.next(1) exhausts 1 term of the sequence, returning 5.  The remaining sequence is now [5].

.next(2) exhausts 2 terms, returning -1.  This is because the first term exhausted was 5,
but the second term did not exist.  Since the last term exhausted does not exist, we return -1.
```

## Solution

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class RLEIterator {
    private int[] nums;
    private int index;
    private int count;

    // 我们的做法，不改变输入进来的数组 nums
    public RLEIterator(int[] nums) {
        this.nums = nums;
        this.index = 0;
        this.count = nums[0];
    }
    
    public int next(int n) {
        // 要防止某一个query已经返回-1以后，后面还有别的query继续来搞事！这是可能的用户行为
        if (index == nums.length) {
            return -1;
        }
        
	// proceed to the next number in the array
	while (n > count && index < nums.length) {
	    n -= count;
	    index += 2;

	    if (index == nums.length) {
		return -1;
            }
	    count = nums[index];
        }
        
        // index < nums.length && n <= count
        count -= n;
        return nums[index + 1];
    }
}
```

## Reference
* [RLE Iterator [LeetCode]](https://leetcode.com/problems/rle-iterator/description/)

{% include links.html %}
