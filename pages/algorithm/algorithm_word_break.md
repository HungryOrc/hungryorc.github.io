---
title: "Word Break"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_word_break.html
folder: algorithm
toc: false
---

## Description
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

Note:
* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

Example 1:

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
Example 2:

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
Example 3:

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

### Example
* Input: 
  * Output: 

## Solution 1: 速度 前33%，很巧妙的 一维DP！左大段，右小段(小段直接用set判断是否含有)
Ref: https://leetcode.com/problems/word-break/discuss/43790/Java-implementation-using-DP-in-two-ways

### Complexity
* Time: O(n^3)
* Space: O(n), dp 数组

### Java
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if (wordDict == null || wordDict.size() == 0) {
            return false;
        }
        if (s.length() == 0) {
            return true;
        }
        
        int n = s.length();
        int maxLen = Integer.MIN_VALUE;
        Set<String> set = new HashSet<>();
        
        for (String str : wordDict) {
            set.add(str);
            
            int len = str.length();
            maxLen = Math.max(maxLen, len);
        }

        // dp[i]是从index=0开始到index=i为止(包含i)，这段substring是否可以被break好
        boolean[] dp = new boolean[n];
        
        // 只做一次遍历
        for (int end = 0; end < n; end++) {
            // substring作为一个整体，不切
            if (end + 1 <= maxLen && set.contains(s.substring(0, end + 1))) {
                dp[end] = true;
                continue;
            }
            
            for (int mid = 0; mid < end; mid++) { // middle index for dividing the substring
                // substring的中间切一刀 
                // 光是取substring的时间消耗就有 O(n) 了
                if (dp[mid] && set.contains(s.substring(mid + 1, end + 1))) {
                    dp[end] = true;
                    break;
                }
            }
        }
        return dp[n - 1];
    }
}
```

## Solution 2: 速度 后10%，笨的 二维DP，仅供存根，别用这个方法

### Complexity
* Time: O(n^3)
* Space: O(n^2), dp 数组

### Java
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if (wordDict == null || wordDict.size() == 0) {
            return false;
        }
        if (s.length() == 0) {
            return true;
        }
        
        int n = s.length();
        int minLen = Integer.MAX_VALUE;
        int maxLen = Integer.MIN_VALUE;
        Set<String> set = new HashSet<>();
        
        for (String str : wordDict) {
            set.add(str);
            
            int len = str.length();
            minLen = Math.min(minLen, len);
            maxLen = Math.max(maxLen, len);
        }
        maxLen = Math.min(maxLen, n); // 不可以比给的String s还长
        
        boolean[][] dp = new boolean[n][n];
        
        // 第一次遍历
        for (int len = minLen; len <= maxLen; len++) { // length of the substring
            for (int i = 0; i + len - 1 < n; i++) { // starting index of the substring
                String substring = s.substring(i, i + len); // 光是取substring的时间消耗就有 O(n) 了
                if (set.contains(substring)) {
                    dp[i][i + len - 1] = true;
                }
            }
        }
        
        // 第二次遍历
        for (int len = minLen; len <= n; len++) { // 注意这里最大长度应该是n了！而非maxLen了
            for (int i = 0; i + len - 1 < n; i++) {
                for (int j = i; j < i + len - 1; j++) {
                    if (dp[i][j] && dp[j + 1][i + len - 1]) {
                        dp[i][i + len - 1] = true;
                        continue;
                    }
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## Reference
* [Word Break [LeetCode]](https://leetcode.com/problems/word-break/description/)

{% include links.html %}
