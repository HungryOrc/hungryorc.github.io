---
title: "Extend Two Strings and Check Their Differences"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_extend_two_strings_and_check_differences.html
folder: algorithm
toc: false
---

## Description
Given 2 strings which has the same initial length, and they will also have the same length after you extend them in the following way:
"2a3b1c" -> "aabbbc", "1a2b3d" -> "abbccc". Return the count of different digits between the 2 extended strings.

Note:
* The numbers can be more than 1 digit
* Both Strings have valid format, namely numbers followed by smaller alphabet letters.

### Example
* Input: "2a3b1c" and "1a2b3c"
  * Output: 3, since the extended strings are: "a(a)b(bb)c" and "a(b)b(cc)c"。用括号标出来了不同的digits。

## Solution
这题看起来简单，先把两个Strings都展开，再一位一位比就行了。
但是如果里面的数字很大的话，就有很大的优化的空间。比如给的两个Strings是 20000a 和 999b1c，那么其实就不必先展开再一位一位比了

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
// helper class。设置它的作用是为了方便在函数之间传递curCount和curChar。这样就不必设global var
class State {
    int curCount;
    char curChar;
}

public class Solution {
    public int countDiff(String s1, String s2) {
        // ignore sanity checks
        
        State state1 = new State();
        State state2 = new State();
        
        int len = s1.length(); // also = s2length()
        int index1 = 0, index2 = 0;
        int diffCount = 0;
        
        // index1 和 index2 未必 “同时” 到达终点，这没问题
        while(index1 < len || index2 < len) {
            if (state1.curCount == 0) {
                index1 = getNextIndexAndUpdateState(s1, index1, state1);
            }
            if (state2.curCount == 0) {
                index2 = getNextIndexAndUpdateState(s2, index2, state2);
            }
            
            int minorCount = Math.min(state1.curCount, state2.curCount);
            
            if (state1.curChar != state2.curChar) {
                diffCount += minorCount;
            }
            
            state1.curCount -= minorCount;
            state2.curCount -= minorCount;
        }
        return diffCount;
    }
    
    private int getNextIndexAndUpdateState(String s, int index, State state) {
        int count = 0;
        char curChar = s.charAt(index);
        
        while (Character.isDigit(curChar)) {
            count = count * 10 + (curChar - '0');
            index++;
            curChar = s.charAt(index);
        }
        
        // 出了while循环的时候，curChar其实已经指向数字后面的那个字母了
        state.curCount = count;
        state.curChar = curChar;
        
        return index + 1; // 为下一个(int + char) 组合做准备
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {

        Solution solu = new Solution();
        int result = solu.countDiff("2a3b1c", "1a2b3c");  // 3
        System.out.println(result);
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
