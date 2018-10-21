---
title: "Erase chars in pairs"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_erase_chars_in_pairs.html
folder: algorithm
toc: false
---

## Description
哦也

### Example
* Input:
  * Output:

## Solution
用二维DP做。boolean dp[i][j]意味着从index i 到 index j 的sub string，左右都inclusive，能否被消除。可见只有i到j是偶数长度的时候才有意义，奇数长度的dp[i][j]都不用去管

### Complexity
* Time: O(n^3)    <=== 对么？
* Space: O(n^2)，二维dp数组

### Java
```java
class Solution {
    public boolean eraseChars(Map<char, Set<char>> pairs, String str) {
        if (str == null || str.length() == 0 || str.length() % 2 == 1) {
            return false;
        }
        
        int n = str.length();
        boolean[][] dp = new boolean[n][n];
        
        for (int i = 0; i <= n - 2; i++) {
            dp[i][i + 1] = isPair(str.charAt(i), str.charAt(i + 1));
        }
        
        for (int len = 4; len <= n; len += 2) {
            for (int i = 0; i + len - 1 <= n - 1; i++) {
                for (int divide = i + 2; divide <= n - 1 - 2; divide += 2) {
                    if (dp[i][divide] && dp[divide + 1][i + len - 1]) {
                        dp[i][i + len - 1] = true;
                        continue;
                    }
                }
            }
        }
        
        return dp[0][n - 1];
    }
    
    private boolean isPair(Map<char, Set<char>> pairs, char left, char right) {
        if (!pairs.containsKey(left)) {
            return false;
        }
        
        if (!pairs.get(left).contains(right)) {
            return false;
        }
        
        return true;
    }
}
```

## Reference
* 网上没有找到这题

{% include links.html %}
