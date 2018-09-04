---
title: "Check if a Ternary Expression is Valid"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_validate_ternary_expression.html
folder: algorithm
toc: false
---

## Description
给一个 String s，它里面只有3种字符：`?`, `:`, `a,b,c...z`。除了`?`和`:`以外，其他chars都是单个不相邻的`a`到`z`之中的chars，即不会出现诸如 `a?bb:c`。
那么要判断这个s是否是一个valid ternary expression。
* The length of the given string can be very long.
* The conditional expressions group right-to-left (as usual in most languages).

### Example
* Input: Input: "c?b:t"
  * Output: True
* Input: "?b:g"
  * Output: False
* Input: "a?b?c:c?:a"
  * Output: False

## Solution
给一个 valid 的 ternary expression string，让你parse它得到它的值。本题的思路诀窍和具体做法都和这一题高度一致。

步骤：
* 从`s`的最右边的char开始，向左走。用一个stack来装这些chars，那么越靠后的char就越接近栈底
  * stack里只装非?也非:的字符
* 遇到`?`就：
  * 从stack里pop出来两个数，首先pop出来的就是`?`之后的第一个char (call it `second char`)，然后pop出来的就是`?`之后紧贴着的那个`:`之后的那个char (call it `third char`)
  * 在`s`里再往左看一个char，这个char就是`?`左边紧贴着的那个char (call it `first char`)
* 这个过程里遇到任何不顺利，比如stack不该空的时候空了，最后stack里不是只剩一个char... 都返回 false
* 如果这个过程ok，那么往stack里push回去一个任意的非?非:的单个char
* 继续在 `s` 里往左走，直到 `s` 的左边第一个char

### Complexity
* Time: O(n)
* Space: O(n), for the stack

### Java
```java
class Solution {
    
    public boolean validateTernary(String expression) {
        if (expression == null || expression.length() == 0) {
            return false;
        }    
        
        char[] chars = expression.toCharArray();
        Stack<Character> stack = new Stack<>();
        
        // 从最右边的一个char开始往左走
        int i = chars.length - 1;
        while (i >= 0) {
            char curChar = chars[i];
            // 只把非?非:的chars放到stack里去
            if (curChar != '?' && curChar != ':') {
                stack.push(curChar);
            } else if (curChar == '?') {
                if (stack.isEmpty()) {
                    return false;
                }
                char secondChar = stack.pop();
                
                if (stack.isEmpty()) {
                    return false;
                }
                char thirdChar = stack.pop();
                
                if (i - 1 < 0) {
                    return false;
                }
                i--;
                char firstChar = chars[i];
            
                // 如果上面的过程ok，那么往stack里push回去一个任意的非?非:的单个char
                stack.push('y'); // y means ok for now
            }
            
            i--; // 无论如何都要往左走一步的
        }
        
        // 最后stack里必须只剩下一个非?非:的char，
        // 才算这个string是valid ternay expression
        return (stack.size() == 1) && 
            (stack.peek() != '?') && 
            (stack.peek() != ':');
    }
}
```

## Reference
* 本题没有找到网上对应题目

{% include links.html %}
