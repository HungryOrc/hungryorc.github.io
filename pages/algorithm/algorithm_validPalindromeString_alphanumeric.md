---
title: "Valid Palindrome String, only Check Letters and Digits"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validPalindromeString_alphanumeric.html
folder: algorithm
toc: false
---

## Description
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.

### Example
* Input: "A man, a plan, a canal: Panama"
  * Output: true
* Input: "*P"
  * Output: true
* Input: "0P"
  * Output: false
* Input: "race a car"
  * Output: false
  
## Solution：双指针一前一后，速度前5%
最直觉的做法。只是要注意：
* 要把大写字母都换成小写，或者反过来
* 对于非字母的char，包括数字char和其他类型的char，都可以使用转大写或转小写的method！用了以后char值不变

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public boolean isPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return true;
        }
        
        int l = 0, r = s.length() - 1;
        
        while (l < r) {
            char cL = Character.toLowerCase(s.charAt(l));
            char cR = Character.toLowerCase(s.charAt(r));
            boolean isAlphaNumL = isAlphaNumeric(cL);
            boolean isAlphaNumR = isAlphaNumeric(cR);
            
            if (isAlphaNumL && isAlphaNumR) {
                if (cL != cR) {
                    return false;
                }
                l++;
                r--;
            } else if (isAlphaNumL) {
                r--;
            } else if (isAlphaNumR) {
                l++;
            } else {
                l++;
                r--;
            }
        }
        return true;
    }
    
    private boolean isAlphaNumeric(char c) {
        if (c >= 'a' && c <= 'z') {
            return true;
        }
        if (c >= 'A' && c <= 'Z') {
            return true;
        }
        if (c >= '0' && c <= '9') { // 注意，这里别用数字0和9，要用char '0'和'9'，这里容易出错
            return true;
        }
        return false;
    }
    // 注意，这个method也可以这么写，利用内置函数 `Character.isLetter(char)` 和 `Character.isDigit(char)`:
    // private boolean isEitherLetterOrDigit(char c) {
    //     return (Character.isLetter(c) || Character.isDigit(c));
    // }    
}
```

## Reference
* [Valid Palindrome [LeetCode]](https://leetcode.com/problems/valid-palindrome/description/)

{% include links.html %}
