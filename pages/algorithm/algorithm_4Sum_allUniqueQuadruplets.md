---
title: "Four Sum, Return All Unique Quadruplets (the Order of the 4 Numbers in a Quadruplet Doesn't Matter)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_4Sum_allUniqueQuadruplets.html                               
folder: algorithm
toc: false
---

## Description
Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:
* The solution set must not contain duplicate quadruplets.
* [1,1,1,2] and [1,1,2,1] are considered as the same quadruplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: nums = [1, 0, -1, 0, -2, 2], and target = 0
  * Output: [[-1,  0, 0, 1], [-2, -1, 1, 2], [-2,  0, 0, 2]]

## Solution 1，先排序数组，然后两边向中间逼近。速度较快

### Complexity
* Time: O(n^3) <=== 对么 ？？？？
* Space: O(1) <=== 对么 ？？？？

### Java
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        HashSet<List<Integer>> dedupResult = new HashSet<>();
        
        if (nums == null || nums.length < 4) {
            return result;
        }
        
        Arrays.sort(nums); // O(nlogn) time
        int n = nums.length;
        // <sum of pair, indexes of pairs that sum up to the key>
        Map<Integer, Set<List<Integer>>> map = new HashMap<>();
        
        for (int i = 0; i < n - 1; i++) { // left index of pair, O(n) time
            for (int j = i + 1; j < n; j++) { // right index of pair, O(n) time
                int sum = nums[i] + nums[j];
                List<Integer> curPair = makePair(i, j);
                
                // case 1: pairs of the same sum as the current pair
                Set<List<Integer>> pairsOfSameSum = map.get(sum);
                
                if (pairsOfSameSum == null) {
                    Set<List<Integer>> set = new HashSet<>();
                    set.add(curPair);
                    map.put(sum, set);
                } 
                else if (sum * 2 != target) {
                    pairsOfSameSum.add(curPair);
                } 
                else { // sum * 2 == target
                    for (List<Integer> pair : pairsOfSameSum) { // O(n) time
                        // the right index of the previous pair must be smaller than
                        // the left index of the current pair
                        if (pair.get(1) < curPair.get(0)) {
                            dedupResult.add(makeQuadruplet(nums, pair, curPair));
                        }
                    }
                    pairsOfSameSum.add(curPair);
                    continue;
                }
                
                // case 2: pairs of the complement sum to the current pair
                Set<List<Integer>> pairsOfComplementSum = map.get(target - sum);
                
                if (pairsOfComplementSum != null) {
                    for (List<Integer> pair : pairsOfComplementSum) {
                        if (pair.get(1) < curPair.get(0)) {
                            dedupResult.add(makeQuadruplet(nums, pair, curPair));
                        }
                    }
                }
            }
        }
        
        for (List<Integer> quadru : dedupResult) {
            result.add(quadru);
        }
        return result;
    }
    
    // helper function
    private List<Integer> makePair(int a, int b) {
        List<Integer> pair = new ArrayList<>();
        pair.add(a);
        pair.add(b);
        return pair;
    }
    
    // helper function
    private List<Integer> makeQuadruplet(int[] nums, List<Integer> l1, List<Integer> l2) {
        List<Integer> quadru = new ArrayList<>();
        for (int index : l1) {
            quadru.add(nums[index]);
        }
        for (int index : l2) {
            quadru.add(nums[index]);
        }
        return quadru;
    }
}
```

## Reference
[4Sum [LeetCode]](https://leetcode.com/problems/4sum/description/)

{% include links.html %}
