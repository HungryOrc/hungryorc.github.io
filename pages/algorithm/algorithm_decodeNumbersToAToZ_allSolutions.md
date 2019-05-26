---
title: "Decode Numbers to A ~ Z: All Solutions"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_decodeNumbersToAToZ_allSolutions.html
folder: algorithm
toc: false
---

## Description
'A'到'Z'这26个大写字母分别代表 1-26 这26个数字。注意没有字母代表0. 给一个char数组，求把它转化成A到Z的字母，共有多少种转化方法。

本题的DP思想的超凡之处在于：**dp数组里的每个元素 不再是一个数字或者boolean，而是一个 List of String！**

因为 Java 里面不能定义 Array of Generic Type，所以也就不能定义 Array of (List of String)，
所以最后我们定义的整个 dp“数组” 是一个 `List<List<String>>`

### Example
略

## Solution: DP
这题有个类似题，只要求求一共有多少种decode的方法，不要求具体哪些solutions，那一题在leetcode上有，叫 Decode Ways I

### Complexity
* Time: O(n)
* Space: O(n), dp 数组

### Java
代码看起来不很短，其实逻辑很简明
```java
public class Solution {
    public List<String> numbersToLetters(char[] chars){
        List<String> result = new ArrayList<>();	
        if (chars == null || chars.length == 0) {
            return result;
        }

        // 如果有任何char不是数字，或者是'0'，都违规，立刻 out
        for (char num : chars) {
            if (num - '0' <= 0 || num - '0' > 9) {
                return result;
            }
        }

        int n = chars.length;
        List<List<String>> dp = new ArrayList<>();

        // 相当于 dp[0]
        List<String> dp0 = new ArrayList<>();
        dp0.add(numToLetter(chars[0]) + "");		
        dp.add(dp0);

        for (int i = 1; i < n; i++) {
            char curChar = numToLetter(chars[i]);
            List<String> dpi = new ArrayList<>();

            // Case 1: 如果当前的最后一位即index=i处的数字被一个字母代表
            for (String s : dp.get(i - 1)) {
                dpi.add(s + curChar);
            }

            // Case 2: 如果当前的最后两位即index=i-1和i处的数字，作为一个整体(两位数)，被一个字母代表
            int lastDigit = chars[i] - '0';
            int secondLastDigit = chars[i - 1] - '0';
            int lastTwoDigits = secondLastDigit * 10 + lastDigit;

            if (lastTwoDigits > 26 || lastTwoDigits == 0) {
                continue;
            }
	    
	    char lastTwoDigitsConvertToChar = numToLetter(lastTwoDigits);
            if (i == 1) {
                dpi.add(lastTwoDigitsConvertToChar + "");			
            } else {
                for (String s : dp.get(i - 2)) {
                    dpi.add(s + lastTwoDigitsConvertToChar);
                }
            }
            dp.add(dpi);
        }
        return dp.get(n - 1);
    }
	
    // 把一个数字（1到26之间）转化为大写字母 'A'到'Z'
    private char numToLetter(int num) {
        return (char)('A' + num - 1);
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
