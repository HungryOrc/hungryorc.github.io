---
title: "Decode Numbers and '*' to A ~ Z"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_decodeNumbersToAToZ_withStar.html
folder: algorithm
toc: false
---

## Description
A message containing letters from A-Z is being encoded to numbers using the following mapping way:
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
Beyond that, now the encoded string can also contain the character '*', which can be treated as one of the numbers from 1 to 9.

Given the encoded message containing digits and the character '*', return the total number of ways to decode it.

Also, since the answer may be very large, you should return the output mod 10^9 + 7.

这题标的是hard，但其实难度差不多是median。

### Example
* Input: "1*"
  * Output: 9 + 9 = 18
    * The first 9 means "1 and 1", "1 and 2", "1 and 3"... "1 and 9"
    * The second 9 means "11", "12", "13"... "19"

## Solution: DP
和 Decode Numbers to A ~ Z 且不含 '*' 那题的思路是一脉相承的。那题在leetcode里叫 Decode Ways I

dp[i] 表示 给的String里从 index=0 开始，到 index=i 为止，这一段chars(可能有'*')，一共有多少种decode成 A ~ Z 的方法。递推方式是：
```java
dp[i] = dp[i - 1] * 一位后缀有多少种decode方式 + dp[i - 2] * 两位后缀有多少种decode方式
      = dp[i - 1] * (index为i的char有多少种decode方式) + 
        dp[i - 2] * (index分别为i-1和i的chars结为一个整体不可分割,这个整体有多少种decode方式)
```
这题并不很难，关键在于，在分析 后缀有多少种decode方式 的时候，要心细！

### Complexity
* Time: O(n)
* Space: O(n)，这个空间可以优化到 O(1)，即只存 dp[i], dp[i - 1], dp[i - 2]

### Java
```java
class Solution {
    private static final int MOD = 1_000_000_007;
    
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        char[] chars = s.toCharArray();
        int n = chars.length;
        long[] dp = new long[n];
        
        // base case 1
        dp[0] = suffixWays(chars[0]);
        // base case 2
        if (n >= 2) {
            // 这里要注意：dp[1] 不等于 suffixWays(chars[0], chars[1])
            // suffixWays(c1, c2) 代表 c1和c2捆绑为一个整体 这一种情况，
            // 而 dp[1] 代表 c1和c2捆绑为一体，以及c1和c2分别独立 这两种情况
            dp[1] = suffixWays(chars[0], chars[1]) +
                    dp[0] * suffixWays(chars[1]);
        }
        
        for (int i = 2; i < n; i++) {
            dp[i] = dp[i - 1] * suffixWays(chars[i]) % MOD +
                    dp[i - 2] * suffixWays(chars[i - 1], chars[i]) % MOD;
            dp[i] %= MOD;
        }
        return (int)dp[n - 1];
    }
    
    private int suffixWays(char c) {
        if (c == '0') return 0;
        if (c == '*') return 9; // 1 ~ 9
        return 1;
    }
    
    private int suffixWays(char c1, char c2) {
        if (c1 == '*' && c2 == '*') { // "**"
            return 15; // 11 ~ 19 and 21 ~ 26
        } else if (c1 == '*' && c2 != '*') { // "*X"
            if (c2 >= '0' && c2 <= '6') return 2;
            return 1;
        } else if (c1 != '*' && c2 == '*') { // "X*"
            if (c1 == '1') return 9;
            else if (c1 == '2') return 6;
            return 0;
        } else { // "XY"
            int value = (c1 - '0') * 10 + (c2 - '0');
            if (value >= 10 && value <= 26) return 1;
            return 0;
        }
    }
}
```

## Reference
* [Decode Ways II [LeetCode]](https://leetcode.com/problems/decode-ways-ii/description/)

{% include links.html %}
