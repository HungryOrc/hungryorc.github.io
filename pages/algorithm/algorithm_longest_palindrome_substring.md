---
title: "Longest Palindrome Substring"
tags: [algorithm, dynamic_programming]
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


<=== 以下需要编辑

/* 方法1：二维DP
The time complexity can be reduced by storing results of subproblems. 
We maintain a boolean table[n][n] that is filled in bottom up manner. 
The value of table[i][j] is true, if the substring is palindrome, otherwise false. 
To calculate table[i][j], we first check the value of table[i+1][j-1], if the value is true and str[i] is same as str[j], 
then we make table[i][j] true. Otherwise false.
时间：O(n^2)，空间：O(n^2)   */

public class Solution {
  
  public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) {
      return "";
    } else if (s.length() == 1) {
      return s;
    }
    
    int n = s.length();
    int maxLen = 1;
    int start = 0, end = 0;
    boolean[][] isPalin = new boolean[n][n];
    
    for (int i = 0; i < n; i++) {
      isPalin[i][i] = true;
    }
    for (int i = 0; i < n - 1; i++) {
      isPalin[i][i + 1] = (s.charAt(i) == s.charAt(i + 1));
      if (maxLen == 1 && isPalin[i][i + 1] == true) {
        maxLen = 2;
        start = i;
        end = i + 1;
      }
    }
    
    for (int len = 3; len <= n; len++) {
      for (int i = 0; i <= n - len; i++) {
        int j = i + len - 1;
        
        if (s.charAt(i) == s.charAt(j) && isPalin[i + 1][j - 1]) {
          isPalin[i][j] = true;
          
          if (len > maxLen) {
            maxLen = len;
            start = i;
            end = j;
          }
        }
      }
    }
    
    return s.substring(start, end + 1);
  }
}


// 方法2：从中间开始，往两边找。先找中间是1个点的，再找中间是2个点的
// 每次往两边扩，左-1，右+1，看还是不是palindrome。如果不是，这个中心点就不必再继续下去了。看下一个中心点
// 这样做法看起来暴力，但时间也是 O(n^2)，和DP方法一样。其实没有任何重复查找，所以不必记忆化任何中间信息 ！！！

// 这个方法的代码看起来有点长，其实思路非常简单明白
public class Solution {
  
  public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) {
      return "";
    } else if (s.length() == 1) {
      return s;
    }
    
    int n = s.length();
    int maxLen = 1;
    int start = 0, end = 0;
    
    // palindrome的中心是一个char
    for (int center = 1; center < n - 1; center++) {
      for (int radius = 1; radius <= center && radius < n - center; radius ++) {
        int i = center - radius;
        int j = center + radius;
        
        if (s.charAt(i) != s.charAt(j)) {
          break;
        } else {
          if (j - i + 1 > maxLen) {
            maxLen = j - i + 1;
            start = i;
            end = j;
          }
        }
      }
    }
    
    // palindrome的中心是2个相同的相邻的chars
    for (int centerL = 1; centerL < n - 1; centerL++) {
      if (s.charAt(centerL) != s.charAt(centerL + 1)) {
        continue;
      } else if (maxLen < 2) {
        maxLen = 2;
        start = centerL;
        end = centerL + 1;
      }
      
      for (int radius = 1; radius <= centerL && radius < n - centerL - 1; radius ++) {
        int i = centerL - radius;
        int j = centerL + 1 + radius;
        
        if (s.charAt(i) != s.charAt(j)) {
          break;
        } else {
          if (j - i + 1 > maxLen) {
            maxLen = j - i + 1;
            start = i;
            end = j;
          }
        }
      }
    }
    
    return s.substring(start, end + 1);
  }
}


<=== 以上需要编辑

## Solution
哦也

### Complexity
* Time: O(?)
* Space: O(?)

### Java
```java

```

## Reference
* [Longest Palindrome Substring [LeetCode]](https://leetcode.com/problems/longest-palindromic-substring/description/)

{% include links.html %}
