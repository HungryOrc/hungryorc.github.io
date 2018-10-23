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
给一个 Map<char, Set<char>>，打个比方map里有个entry是 <'a', ['b', 'c']> 的话，意味着如果 "ab" 挨在一起，或者 “ac” 挨在一起，
那么它们是可以消掉的。比如 “abe” 会消成 “e”，“eacf” 会消成 “ef”。“aabc”可以先消成“ac”，再完全消掉。
 
有的时候为了整体消掉，先遇到的pair也可以先不消。比如 "abdc"，如果ab先消掉了，dc无法消，这个string就失败了。但如果ab能消而不消，看看bd能否消，
如果bd能消，那么剩下ac，ac也能消，那么整个string就消成功了。

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
        
        // 先针对长度为2的所有substring进行dp录入
        for (int i = 0; i <= n - 2; i++) {
            dp[i][i + 1] = isPair(str.charAt(i), str.charAt(i + 1));
        }
        
        // substring的长度是每次加2
        for (int len = 4; len <= n; len += 2) {
            // substring的起始位置是每次加1！不是加2！
            for (int i = 0; i + len - 1 <= n - 1; i++) {
                // 分隔位置从起始位置后2位开始，每次加2
                for (int divide = i + 2 - 1; （i + divide - 1）+ 2 <= n - 1; divide += 2) {
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
