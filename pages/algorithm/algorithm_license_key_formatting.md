---
title: "License Key Formatting"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_license_key_formatting.html
folder: algorithm
toc: false
---

## Description
You are given a license key represented as a string S which consists only alphanumeric character and dashes. The string is separated into N+1 groups by N dashes.

Given a number K, we would want to reformat the strings such that each group contains exactly K characters, except for the first group which could be shorter than K, but still must contain at least one character. Furthermore, there must be a dash inserted between two groups and all lowercase letters should be converted to uppercase.

Given a non-empty string S and a number K, format the string according to the rules described above.

Note:
* The length of string S will not exceed 12,000, and K is a positive integer.
* String S consists only of alphanumerical characters (a-z and/or A-Z and/or 0-9) and dashes(-).
* String S is non-empty.

### Example
* Input: S = "5F3Z-2e-9-w", K = 4
  * Output: "5F3Z-2E9W"
* Input: S = "2-5g-3-J", K = 2
  * Output: "2-5G-3J"
* Input: S = "2", K = 2
  * Output: "2"

## Solution，这题是Easy题，但有一些corner cases 还是要小心

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        if (s == null || s.length() == 0) {
            return s;
        }
        
        StringBuilder sb = new StringBuilder();
        
        for (char c : s.toCharArray()) {
            if (c == '-') {
                continue;
            }
            sb.append(Character.toUpperCase(c));
        }
        s = sb.toString();
        sb = new StringBuilder();
        
        int n = s.length();
        int firstSectionLen = n % k;
        
        if (firstSectionLen > 0) {
            for (int i = 0; i < firstSectionLen; i++) {
                char c = s.charAt(i);
                sb.append(c);
            }
            
            if (n > firstSectionLen) sb.append('-');
        }
        
        for (int i = firstSectionLen; i < n; i++) {
            char c = s.charAt(i);
            sb.append(c);
            if ((i + 1 - firstSectionLen) % k == 0 && i < n - 1) {
                sb.append('-');
            }
        }
        return sb.toString();
    }
}
```

## Reference
* [License Key Formatting [LeetCode]](https://leetcode.com/problems/license-key-formatting/description/)

{% include links.html %}
