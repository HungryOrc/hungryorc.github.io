---
title: "标题在这里"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_decode_numbers_to_letters_number_of_ways.html
folder: algorithm
toc: false
---

## Description
'A'到'Z'这26个大写字母分别代表 1-26 这26个数字。注意没有字母代表0. 给一个char数组，数组里的char都是'1'到'9'。求把它转化成A到Z的字母，共有多少种转化方法。

### Example
* Input: 12
  * Output: 可以转化为 A,B（1,2），L（12），一共2种方法
* Input: 123
  * Output: 可以转化为 A,B,C（1,2,3），L,C（12,3），A,W（1,23），一共3种方法
* Input: 2119
  * Output: 可以转化为 B,A,A,I（2,1,1,9），U,A,I（21,1,9），U,S（21,19），B,K,I（2,11,9），B,A,S（2,1,19），一共5种方法

## Solution 1: DP
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

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
