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
所以最后我们定义的整个 dp“数组” 是一个 List<List<String>>

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
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        char[] cArray = s.toCharArray();
        int len = cArray.length;
        int[] dp = new int[len];
        
        // we do have len >= 1
        if (isValidOneDigit(cArray[0])) {
            dp[0] = 1;
        } else {
            return 0;
        }
        
        if (len >= 2) {
            if (isValidOneDigit(cArray[1])) {
                dp[1] ++;
            }
            if (isValidTwoDigits(cArray[0], cArray[1])) {
                dp[1] ++;
            }            
        }
        
        // if len >= 3
        for (int i = 2; i < len; i++) {
            if (isValidOneDigit(cArray[i])) {
                dp[i] += dp[i - 1];
            }
            if (isValidTwoDigits(cArray[i - 1], cArray[i])) {
                dp[i] += dp[i - 2];
            }
        }
        
        return dp[len - 1];
    }
    
    private boolean isValidOneDigit(char c) {
        return (c - '0' >= 1) && (c - '0' <= 9);
    }
    
    private boolean isValidTwoDigits(char c1, char c2) {        
        int num = 0;
        num += (c1 - '0') * 10;
        num += c2 - '0';
        
        return num >= 10 && num <= 26;
    }
}
```

## Solution 2: Recursion，很慢！几乎是倒数 1%，不要用这个方法
这题用 Recursive way 慢的原因是：比如数组中间的一个两位数，每次别人经过它的时候都要判断一次它是否valid，如果它左边一共有x位，一共有了y种valid way，那么在它这里就要搞y次验证

### Complexity
* Time: O(2^n)  <=== 对么？
* Space: O(n)，call stack应该是n层

### Java
```java
class Solution {
    int numOfWays = 0;
    
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        char[] cArray = s.toCharArray();
        dfs(cArray, 0);
        return numOfWays;
    }
    
    private void dfs(char[] cArray, int curIndex) {
        if (curIndex == cArray.length) {
            numOfWays++;
            return;
        }
        
        if (validSingleDigit(cArray, curIndex)) {
            dfs(cArray, curIndex + 1);
        }
        if (validDoubleDigits(cArray, curIndex)) {
            dfs(cArray, curIndex + 2);
        }
    }
    
    private boolean validSingleDigit(char[] cArray, int curIndex) {
        char curChar = cArray[curIndex];
        
        // 'A' was marked as 1 in this question, not 0,
        // so only 9 chars starting with 'A' are represented by one digit number
        return (curChar - '0' >= 1) && (curChar - '0' <= 9);
    }
    
    private boolean validDoubleDigits(char[] cArray, int curIndex) {
        if (curIndex > cArray.length - 2) {
            return false;
        }
        
        // 两位里的十位不能是0！是0就是invalid的两位数
        if (cArray[curIndex] == '0') {
            return false;
        }
        
        int number = 0;
        number += (cArray[curIndex + 1] - '0');
        number += (cArray[curIndex] - '0') * 10;
        return (number >= 10) && (number <= 26);
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
