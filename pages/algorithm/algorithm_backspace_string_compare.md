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
* "ab###" = ""，这个String其实backspace的个数过多了，就是说**String的左端的chars被删光了以后，backspaces的个数还没用完**。按照这题的要求，这种情况按照最终为 空String 处理。而不是按照 invalid String 来处理

Note:
* 1 <= S.length <= 200
* 1 <= T.length <= 200
* S and T only contain lowercase letters and '#' characters

Can you solve it in O(N) time **and O(1) space**?

### Example
* Input: S = "ab#c", T = "ad#c"
  * Output: True
* Input: S = "ab##cc#d", T = "cd"
  * Output: True
* Input: S = "a#", T = "a##"
  * Output: True
  
## Solution：双指针 从右往左！速度 前1%
如果不要求in place解决，那么非常简单。如果要求in place，则费不少功夫，**窍门是 从右往左 撸String**。每个String各用一个index，就是说一共是2个指针。

注意：
* 如果2个Strings的 指针 同时到达 -1，则返回 true
* 如果一个指针到了-1，另一个还没到，也不能断定这二者就不相等！因为后者从当前位置开始到最左端最后都能完全消掉的话，那么这两个string也是相等的。比如：
  * "a#bbb##" 和 "bbb##" 其实是相等的，但后者的指针会先到 -1。同理，"a#bbb##" 和 "bb#" 也是相等的
  * 如果到最后，一个指针到了-1，另一个无法到达-1，则返回 false
* 如果2个指针都 >= 0 的时候，就发现它们指向不相等的chars，则返回 false

### Complexity
* Time: O(n)
* Space: O(1)

### Java
代码虽然看起来有点长，其实思路很简明。只是细节情况有点多，要分别处理好
```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        if ((S == null && T == null) || (S.length () == 0 && T.length() == 0)) {
            return true;
        }
        
        int is = S.length() - 1;
        int it = T.length() - 1;
        
        while (is >= 0 && it >= 0) {
            is = getNextIndex(S, is);
            it = getNextIndex(T, it);
            
            if (is == -1 && it == -1) {
                return true;
            } else if (is == -1 || it == -1) {
                return false;
            }
            
            if (S.charAt(is) == T.charAt(it)) {
                is--;
                it--;
            } else {
                return false;
            }
        }
        
        if (is == -1 && it == -1) {
            return true;
        }
        if (is == -1 && getNextIndex(T, it) == -1) {
            return true;
        }
        if (it == -1 && getNextIndex(S, is) == -1) {
            return true;
        }
        return false;
    }
    
    private int getNextIndex(String str, int i) {
        int countOfBackspaces = 0;
        
        while (i >= 0) {
            if (str.charAt(i) == '#') {
                countOfBackspaces++;
                i--;
            } else { // != '#'
                if (countOfBackspaces == 0) {
                    return i;
                } else {
                    countOfBackspaces--;
                    i--;
                }
            }
        }
        return -1;
    }
}
```

## Reference
* [Backspace String Compare [LeetCode]](https://leetcode.com/problems/backspace-string-compare/description/)

{% include links.html %}
