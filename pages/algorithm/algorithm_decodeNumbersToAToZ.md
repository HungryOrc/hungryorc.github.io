---
title: "Decode Numbers to A ~ Z: Number of Ways"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_decodeNumbersToAToZ.html
folder: algorithm
toc: false
---

## Description
'A'到'Z'这26个大写字母分别代表 1-26 这26个数字。注意没有字母代表0，但是有10和20. 给一个char数组，数组里的char都是'1'到'9'。求把它转化成A到Z的字母，共有多少种转化方法。

可以认为给出的char array是valid的。即它里面只有数字'0'到'9'。然后整个数组的第一个元素不会是0，否则无法decode；中间如果出现0是可以decode的，但是中间只能出现10和20，不能出现别的X0。

### Example
* Input: 12
  * Output: 可以转化为 A,B（1,2），L（12），一共2种方法
* Input: 123
  * Output: 可以转化为 A,B,C（1,2,3），L,C（12,3），A,W（1,23），一共3种方法
* Input: 2119
  * Output: 可以转化为 B,A,A,I（2,1,1,9），U,A,I（21,1,9），U,S（21,19），B,K,I（2,11,9），B,A,S（2,1,19），一共5种方法

## Solution 1: DP，速度前 1%
int[] dp = new int[charArray.length]，dp[i] 的意思是 要表示 从index=0到index=i的所有的chars，一共有多少种表示方法。
* 初始：dp[0] = 1
* 递推：dp[i] = dp[i - 1] + dp[i - 2]
  * 加号左边的一项 dp[i - 1] 是一定存在的，这一项意味着当前的最后一位即index=i处的数字被一个字母代表
  * 加号右边的一项 dp[i - 2] 未必存在，这一项意味着当前的最后两位即index=i-1和i处的数字，作为一个两位数，被一个字母代表，那么可以看出，这一项存在的条件是，index=i-1和i这两个数字综合起来的两位数必须 <= 26，才可能被一个字母来表示

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
* [Decode Ways [LeetCode]](https://leetcode.com/problems/decode-ways/description/)

{% include links.html %}
