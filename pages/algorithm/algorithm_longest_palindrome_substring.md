---
title: "Longest Palindrome Substring"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longest_palindrome_substring.html
folder: algorithm
toc: false
---

## Description
Given a string S, find the longest palindromic substring in S.
* There exists one unique longest palindromic substring.    
* The input S is not null.

### Example
* Input: "abbc"
  * Output: "bb"

## Solution：从中间开始，往两边找。先找中间是1个点的，再找中间是2个点的。速度前45%
每次往两边扩，左-1，右+1，看还是不是palindrome。如果不是，这个中心点就不必再继续下去了。看下一个中心点

这样做法看起来暴力，其实时间是 O(n^2)，是最优解。因为 **这种做法其实没有任何重复查找，所以不必记忆化任何中间信息。所以这题根本不需要DP的做法！** 也不需要搞一个DP矩阵来浪费空间

### Complexity
* Time: O(N^2)
* Space: O(1)

### Java
代码看起来有点长，其实思路很简明
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return s;
        }
        
        int n = s.length();
        int finalStart = 0, finalEnd = 0;
        int start = 0, end = 0;

        for (int mid = 1; mid < n - 1; mid++) {
            for (int radius = 1; radius <= n / 2; radius++) {
                if (mid - radius < 0 || mid + radius == n) {
                    break;
                }
                
                start = mid - radius; 
                end = mid + radius;
                if (s.charAt(start) == s.charAt(end)) {
                    if ((end - start) > (finalEnd - finalStart)) {
                        finalStart = start;
                        finalEnd = end;
                    }
                } else {
                    break;
                }
            }
        }

        // 这里从radius=0开始！意味着考察2个相邻的char
        for (int mid = 0; mid < n - 1; mid++) {
            for (int radius = 0; radius <= n / 2; radius++) {
                if (mid - radius < 0 || mid + 1 + radius == n) {
                    break;
                }
                
                start = mid - radius;
                end = mid + 1 + radius;
                if (s.charAt(start) == s.charAt(end)) {
                    if ((end - start) > (finalEnd - finalStart)) {
                        finalStart = start;
                        finalEnd = end;
                    }
                } else {
                    break;
                }
            }
        }
        return s.substring(finalStart, finalEnd + 1);
    }
}
```

## Reference
* [Longest Palindrome Substring [LeetCode]](https://leetcode.com/problems/longest-palindromic-substring/description/)

{% include links.html %}
