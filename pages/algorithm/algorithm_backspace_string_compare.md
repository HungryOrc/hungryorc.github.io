---
title: "Backspace String Compare"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backspace_string_compare.html
folder: algorithm
toc: false
---

## Description
Given two strings S and T, return if they are equal when both are typed into empty text editors. # means a backspace character. For example:
* "ab#c" = "ac"
* "abb#c###" = ""
* "ab###" = invalid string

Note:
* 1 <= S.length <= 200
* 1 <= T.length <= 200
* S and T only contain lowercase letters and '#' characters
* 这些Strings **有可能 invalid！**

Can you solve it in O(N) time **and O(1) space**?

### Example
* Input: S = "ab#c", T = "ad#c"
  * Output: True
* Input: S = "ab##cc#d", T = "cd"
  * Output: True

## Solution
如果不要求in place解决，那么非常简单。如果要求in place，则费不少功夫，**窍门是 从右往左 撸string**

注意如下的特殊情况（以下讨论都给予从后往前处理这两个string）：
* 一个string分析完了，另一个string还没分析完，这二者已分析的部分都是相等的。到此不能断定这二者整体来说就不相等！因为后者从当前位置开始到最左端最后都能完全消掉的话，那么这两个string也是相等的

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int iS = S.length() - 1;
        int iT = T.length() - 1;
        int stepBackS = 0;
        int stepBackT = 0;

        // iS和iT同步到达-1则2个string相同，但不同步到达-1也代表2个string相同！
        // 要是一个到了-1，另一个永远到不了-1，则false；
        // 或者两个都在>=0的时候就发现了不同的char，则false
        while (iS >= 0 && iT >= 0) {
            iS = getNextIndexForCompare(S, iS);
            iT = getNextIndexForCompare(T, iT);
            
            if (iS >= 0 && iT >= 0) {
                if (S.charAt(iS) != T.charAt(iT)) {
                    return false;
                }
                iS--;
                iT--;
            }
        }
        
        if (iS == -1 && getNextIndexForCompare(T, iT) >= 0) {
            return false;
        }
        if (iT == -1 && getNextIndexForCompare(S, iS) >= 0) {
            return false;
        }
        // iT == -1 && iS == -1
        return true;
    }
    
    private int getNextIndexForCompare(String str, int curIndex) {
        // 如果当前已经超出了左界，则一直停在这里
        if (curIndex == -1) {
            return -1;
        }
        
        int moveLeft = 0;
        while (moveLeft > 0 || str.charAt(curIndex) == '#') {
            if (str.charAt(curIndex) == '#') {
                moveLeft++;
            } else if (moveLeft > 0) { // 而且当前char不是'#'
                moveLeft--;
            }
            
            curIndex--;
            
            if (curIndex == -1) {
                return -1;
            }
        }
        return curIndex;
    }
}
```

## Reference
* [Backspace String Compare [LeetCode]](https://leetcode.com/problems/backspace-string-compare/description/)

{% include links.html %}
