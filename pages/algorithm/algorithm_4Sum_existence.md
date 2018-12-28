---
title: "Four Sum, Check If There Exists a Quadruplet that Sum up to the Target Number"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_4Sum_existence.html                               
folder: algorithm
toc: false
---

## Description
Determine if there exists a set of four elements in a given array that sum to the given target number.

Note:
* The given array is not null and has length of at least 4
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: {1, 2, 2, 3, 4}, target = 9
  * Output: True. 1 + 2 + 2 + 4 = 9

## Solution：把4个数化为2个Pair！然后用瞄准一个pair的loop来实现模拟两个pairs！    <=== 对么 ？？？？ only tested this code in Eclipse, no online trial

用 HashMap 以及 双层 for loop

把4个数想象为2个pair，这4个数就分别是 p1L, p1R, p2L, p2R. 它们的和需要等于特定的target。
然而还有一个重要的要求，就是 p2L 的index 必须 > p1R 的index

### Complexity
* Time: O(n^2) <=== 对么 ？？？？
* Space: O(n)，map <=== 对么 ？？？？

### Java
```java
class Pair {
    int leftIndex, rightIndex;
    public Pair(int li, int ri) {
	this.leftIndex = li;
	this.rightIndex = ri;
    }
}

public class Solution {
    public boolean fourSum(int[] nums, int target) {
	if (nums == null || nums.length < 4) {
	    return false;
	}

	int n = nums.length;
	// key is the sum of the 2 numbers in the pair
	Map<Integer, Pair> map = new HashMap<>();

	for (int i = 0; i < n - 1; i++) { // index of the left number in the pair
	    for (int j = i + 1; j < n; j++) { // index of the right number in the pair
		int sum = nums[i] + nums[j];

		Pair sameSumPair = map.get(sum);
		if (sameSumPair != null) {
		    if (sum * 2 == target && sameSumPair.rightIndex < i) {
		        return true;
		    } else {
			continue;
		    }
		}

		Pair complementPair = map.get(target - sum); // complement pair
		// if the complement pair exists, and its right index is smaller than
		// the left index of the current pair, then we found a valid quadruplet
		if (complementPair != null && complementPair.rightIndex < i) {
		    return true;
		}

		map.put(sum, new Pair(i, j));
	    }
	}
	return false;
}


public static void main(String[] args) {
	int[] nums = {1, 1, 2, 5, 11, 20, 4, -1};
	int target = 37;

	Solution solu = new Solution();

	System.out.println(solu.fourSum(nums, target));
    }
}
```

## Reference
Did not find this question online

{% include links.html %}
