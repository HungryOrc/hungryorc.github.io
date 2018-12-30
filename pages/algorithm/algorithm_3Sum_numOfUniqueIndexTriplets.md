---
title: "3 Sum, Return All Unique Triplets (the Order of the 3 Numbers in a Triplet Doesn't Matter)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_3Sum_numOfUniqueIndexTriplets.html                               
folder: algorithm
toc: false
---

## Description
注意这题 **要返回的是 unique triplets of indexes**，不是 triplets of element values。而 indexes 是永远不会重复的，所以 triplets of indexes 也永远不会重复，不管 element values 是否有重复。
* 除非故意把 index triplet 的顺序打乱，比如 indexes [1,2,3] 和 [1,3,2] 是重复的，但我们按照index从小到大的顺序来做，也不会出现这种情况

Given an integer array A, and an integer target, return the number of tuples i, j, k  such that i < j < k and A[i] + A[j] + A[k] == target.
* 3 <= A.length <= 3000
* 0 <= A[i] <= 100
* 0 <= target <= 300
* 最后

As the answer can be very large, return it modulo 10^9 + 7. 这是这一题的特殊要求，和主干逻辑无关，其他类似的题也不需要做这个

Note:
* The solution set must not contain duplicate triplets of indexes.
* [1,1,2] and [2,1,1] and [1,2,1] (all the numbers here are indexes) are considered as the same triplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: A = [1,1,2,2,3,3,4,4,5,5], target = 8
  * Output: 20
  * Enumerating by the values (A[i], A[j], A[k]):
    ```
    (1, 2, 5) occurs 8 times;
    (1, 3, 4) occurs 8 times;
    (2, 2, 4) occurs 2 times;
    (2, 3, 3) occurs 2 times.
    ```
* A = [1,1,2,2,2,2], target = 5
  * Output: 12
  * A[i] = 1, A[j] = A[k] = 2 occurs 12 times:
    ```
    We choose one 1 from [1,1] in 2 ways,
    and two 2s from [2,2,2,2] in 6 ways.
    ```

## Solution：DP，我自己的想法，感觉思路是比较给力的，但实测速度很慢，还没看leetcode的答案

### Complexity
* Time: O(n^2)
* Space: O(1) <=== 对么 ？？？？

### Java
```java
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums); // O(nlogn) time
        List<List<Integer>> result = new ArrayList<>(); 
        
        // the 1st number in the triplet
        for (int i = 0; i < nums.length - 2; i++) {
            
            // dedupte the 1st number
            if (i == 0 || (i > 0 && nums[i] != nums[i - 1])) {
                int start = i + 1, end = nums.length - 1;
                int target = 0 - nums[i];
                
                while (start < end) {
                    if (nums[start] + nums[end] == target) {
                        result.add(Arrays.asList(nums[i], nums[start], nums[end]));
                        
                        // 这两个while loop很重要！不是可有可无的！triplet里的后面两个数的去重就靠它了！！
                        while (start < end && nums[start] == nums[start + 1]) {
                            start++;
                        }
                        while (start < end && nums[end] == nums[end - 1]) {
                            end--;
                        }
                        
                        start++;
                        end--;
                    } else if (nums[start] + nums[end] < target) {
                        start++;
                    } else {
                        end--;
                    }
                }
            }
        }
        return result;
    }
}
```

## Solution 2，把数组排序，然后一个 for loop 包一个2Sum，2Sum 用 HashSet 做。实测速度非常慢

### Complexity
* Time: O(n^2)
* Space: O(n)，size of set

### Java
这个代码看起来不短，其实逻辑很简单
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length < 3) {
            return result;
        }
        
        int n = nums.length;
        Arrays.sort(nums); // O(nlogn) time
        
        // i indicates the 1st number in the triplet
        for (int i = 0; i < n - 2; i++) {
            
            // deduplicate the 1st number
            if (i >= 1 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            List<List<Integer>> pairs = twoSum(nums, i + 1, 0 - nums[i]);

            for (List<Integer> pair : pairs) {
                // in this way we are appending the 1st number last,
                // but doesn't matter, deduplication had been ensured beforehand
                pair.add(nums[i]);
                result.add(pair);
            }
        }
        return result;
    }
    
    // the input array is guaranteed to be sorted
    private List<List<Integer>> twoSum(int[] nums, int start, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        boolean foundTwoHalvesOfTarget = false;
        
        for (int i = start; i < nums.length; i++) {
            int cur = nums[i];
            
            if (set.contains(cur)) {
                if (cur * 2 == target && !foundTwoHalvesOfTarget) {
                    foundTwoHalvesOfTarget = true;
                    List<Integer> pair = new ArrayList<>();
                    pair.add(cur);
                    pair.add(cur);
                    result.add(pair);
                }
                continue;
            }

            if (set.contains(target - cur)) {
                List<Integer> pair = new ArrayList<>();
                pair.add(cur);
                pair.add(target - cur);
                result.add(pair);
            }
            set.add(cur);
        }
        return result;
    }
}
```

## Reference
[3Sum [LeetCode]](https://leetcode.com/problems/3sum/description/)

{% include links.html %}
