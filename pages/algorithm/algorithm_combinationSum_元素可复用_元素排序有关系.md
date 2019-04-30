---
title: "Combination Sum，元素可复用，元素排序有关系 (更像是 Permutation Sum)"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_combinationSum_元素可复用_元素排序有关系.html
folder: algorithm
toc: false
---

## Description
* 数组里的元素不重复（但就算有重复也无所谓，因为这一题允许元素复用）
* 在一个组合里，元素可以复用
* 在一个组合里，总的元素个数不限
* 在一个组合里，每个元素的出现顺序如果改变，就视为新的组合！！

Given an integer array with all positive numbers and no duplicates, 
find the number of possible combinations that add up to a positive integer target.

### Follow up
* What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?

### Example
* Input: candidates = [1, 2, 3], target = 4,
  * Output: all the possible combinations (actually permuations) are as follows:
  * 注意, 不同的顺序算是不同的组合！比如 (1, 1, 2) 和 (1, 2, 1) 算是2种组合
  ```
  (1, 1, 1, 1)
  (1, 1, 2)
  (1, 2, 1)
  (1, 3)
  (2, 1, 1)
  (2, 2)
  (3, 1)
  ```

## Solution 1: Pure DFS Recursion, 太慢了, so only passes some of the test cases, the others are timed out
每当要加入一个新的数的时候，都是把数组 nums 里的每一个数都拿出来试一下，每一个数都有平等的在这一位出场的权力

### Complexity
* Time: O(n^n) <=== 对么？？？
* Space: O(n)，每个list的可能size

### Java
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
    	
        int[] result = {0};
        Arrays.sort(nums);
        dfs(nums, target, result);
        return result[0];
    }

    private void dfs(int[] nums, int remain, int[] result) {
        if (remain == 0) {
            result[0]++;
            return;
        }

        for (int num : nums) {
            if (num > remain) {
                return;
            }
	
            dfs(nums, remain - num, result);
        }
    }
}
```

## Solution 2: DP, adding from smaller sum to bigger sum, speed top 15%

### Complexity
* Time: O(target * n), n is the number of elements in the `nums` array. We are also doing a sorting for the `nums` array, so if nlogn > target x n, then the time complexity is nlogn
* Space: O(target), size of the dp array

### Java
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        Arrays.sort(nums);
        int[] dp = new int[target + 1]; // from 0 to target
        dp[0] = 1; // this means put nothing into the combination, it's still one solution
        
        for (int sum = 1; sum <= target; sum++) {
            for (int num : nums) {
                
                if (num > sum) {
                    break;
                }
                
                dp[sum] += dp[sum - num];
            }
        }
        return dp[target];
    }
}
```

## Solution 3: 一种非常奇葩的DP做法，从大到小算，tricky。看下就好，不便运用。speed top 15%

### Complexity
* Time: ????
* Space: O(target), size of the dp array

### Java
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        Arrays.sort(nums);
        
        int[] dp = new int[target + 1]; // from 0 to target
        Arrays.fill(dp, -1); // -1 means un-touched, 0 means no way
        dp[0] = 1; // this means put nothing into the combination, it's still one solution
        
        return dfs(nums, target, dp);
    }
    
    private int dfs(int[] nums, int target, int[] dp) {
        if (dp[target] != -1) { // reuse the memorized data
            return dp[target];
        }
        
        int result = 0;
        
        for (int num : nums) {
            if (num > target) {
                break;
            }
            
            // dp[target] might interfere with others in the process,
            // so it is safe to set a result variable during the process
            result += dfs(nums, target - num, dp);
        }
        
        dp[target] = result;
        return result; 
    }
}
```

## Reference
* [Combination Sum IV [LeetCode]](https://leetcode.com/problems/combination-sum-iv/)

{% include links.html %}
