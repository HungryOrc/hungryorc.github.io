---
title: "Distinct Subsequences II"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_distinct_subsequences_ii.html
folder: algorithm
toc: false
---

## Description
Given a string S, count the number of distinct, non-empty subsequences of S .

Since the result may be large, return the answer modulo 10^9 + 7.

Note:
* S contains only lowercase letters.
* 1 <= S.length <= 2000

### Example
* Input: "abc"
  * Output: 7. The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".
* Input: "aba"
  * Output: 6. The 6 distinct subsequences are "a", "b", "ab", "ba", "aa" and "aba".

## Solution
思路并不好想。答案的思路在此：https://leetcode.com/problems/distinct-subsequences-ii/solution/



### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    private static final int MOD = 1_000_000_007;
    
    public int distinctSubseqII(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int n = s.length();
        int[] dp = new int[n]; // 从index=0到index=i这一段（未必在i处结尾）共有多少种不同的sub sequences
        Map<Character, Integer> lastOccurIndex = new HashMap<>();
        
        dp[0] = 2; // 什么都没有，以及放一个首字母，这两种
        lastOccurIndex.put(s.charAt(0), 0);
        
        for (int i = 1; i < n; i++) {
            char c = s.charAt(i);
            
            Integer lastOccurIndexOfC = lastOccurIndex.get(c);
            int duplicateCount = 0;
            if (lastOccurIndexOfC == null) { // c 之前没出现过
                duplicateCount = 0;
            } else if (lastOccurIndexOfC == 0) { // c 上一次出现是在s的头部第一个字母处
                duplicateCount = 1; // 这个1是指什么都没有的时候的那个空sub sequence。或者说是用 2 * 2 - 3 = 1 反推得来的
            } else {
                duplicateCount = dp[lastOccurIndexOfC - 1];
            }
            
            dp[i] = (2 * dp[i - 1]) % MOD - (duplicateCount) % MOD;
            
            // 特别注意！有可能duplicateCount然后就把dp[i]给减成负数了！
            if (dp[i] <= 0) dp[i] += MOD;
            
            lastOccurIndex.put(c, i);
        }
        
        //if (dp[n - 1] < MOD)  return dp[n - 1] + MOD - 1;
        return dp[n - 1] % MOD - 1;
    }
} 
```

## Reference
* [Distinct Subsequences II [LeetCode]](https://leetcode.com/problems/distinct-subsequences-ii/description/)

{% include links.html %}
