---
title: "Parse a Valid Ternary Expression"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_parse_valid_ternary_expression.html
folder: algorithm
toc: false
---

## Description
Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. 
You can always assume that the given expression is valid and only consists of digits 0-9, ?, :, T and F 
(T and F represent True and False respectively).
* The length of the given string can be very long.
* Each number will contain only one digit.
* The conditional expressions group right-to-left (as usual in most languages).
* The condition will always be either T or F. That is, the condition will never be a digit.
* The result of the expression will always evaluate to either a digit 0-9, T or F.

### Example
* Input: Input: "T?2:3"
  * Output: "2"
* Input: "F?1:T?4:5"
  * Output: "4"
  * The conditional expressions group right-to-left, it is read/evaluated as:
    ```
       "(F ? 1 : (T ? 4 : 5))"                   "(F ? 1 : (T ? 4 : 5))"
    -> "(F ? 1 : 4)"                 or       -> "(T ? 4 : 5)"
    -> "4"                                    -> "4"
    ```
* Input: "T?T?F:5:3"
  * Output: "F"
  * The conditional expressions group right-to-left, it is read/evaluated as:
    ```
       "(T ? (T ? F : 5) : 3)"                   "(T ? (T ? F : 5) : 3)"
    -> "(T ? F : 3)"                 or       -> "(T ? F : 5)"
    -> "F"                                    -> "F"
    ```

## Solution
已知这个 given String (设为`s`) 是valid的。一个valid的 ternary expression 必然有一个或多个这种 `5-char pattern`：`x?y:z`，它(们)是这个 ternary expression 的核心(们)，从它(们)开始向其左右不断扩张，就能最终破解整个 ternary expression。

但是对于同一个String来说，可以存在不止一种valid的 `5-char pattern` 的切分方式，比如对于 `b?e?a:h?f:c:g`，以下2种切法都是valid的！
* `b?(e?a:(h?f:c)):g`
* `b?((e?a:h)?f:c):g`

那么如何找到至少一种valid的切法，就是如何找到至少一个valid的“核心”，就是如何找到至少一个valid的**最右边**的”核心“（上面的例子可以看出，所谓的”最右边“的核心，也可以不止一种的，都是valid的）。

那么我们就可以**从String的最右边开始，向左走，遇到?号就取它右边的2个char，再取它左边的1个char (无视所有的:符号)**。这样就能得到至少一种valid的最右边的核心。这种算法之所以ok，是因为：一个?后面可能紧接一个?，但是最右边的那个?的后面一定只能再接一个:了，因为之前都说死了它就是最右边的那个?了哈哈哈。就上面例子里的2种切割方法来说，这种算法能得到的是第一种切割方法的最右边的核心。

步骤：
* 从`s`的最右边的char开始，向左走。用一个stack来装这些chars，那么越靠后的char就越接近栈底
* 遇到`?`就：
  * 从stack里pop出来两个数，首先pop出来的就是`?`之后的第一个char (call it `second char`)，然后pop出来的就是`?`之后紧贴着的那个`:`之后的那个char (call it `third char`)
  * 在`s`里再往左看一个char，这个char就是`?`左边紧贴着的那个char (call it `first char`)
  * 有了这3个char，就可以推出这个5-char组合的值，要么是 second char，要么是 third char
  * 把这个结果push回到stack里去
* 继续在 `s` 里往左走，直到 `s` 的左边第一个char

### Complexity
* Time: O(n)
* Space: O(n), for the stack

### Java
```java
class Solution {
    
    public String parseTernary(String expression) {
        if (expression == null || expression.length() == 0) {
            return null;
        }    
        
        char[] chars = expression.toCharArray();
        Stack<Character> stack = new Stack<>();
        
        // 从最右边的一个char开始往左走
        int i = chars.length - 1;
        while (i >= 0) {
            char curChar = chars[i];
            // T, F, or 0-9. 只把这些char放到另一个stack里去
            if (curChar != '?' && curChar != ':') {
                stack.push(curChar);
            } else if (curChar == '?') {
                char secondChar = stack.pop();
                char thirdChar = stack.pop();

                i--;
                char firstChar = chars[i];
            
            if (firstChar == 'T') {
                stack.push(secondChar);
            } else { // firstChar == 'F'
                stack.push(thirdChar);
            }
            
            i--; // 无论如何都要往左走一步的
        }
        
        char finalResult = stack.pop(); // 注意！别忘了这个！
        return Character.toString(finalResult);
    }
}
```

## Reference
* [Ternary Expression Parser [LeetCode]](https://leetcode.com/problems/ternary-expression-parser/description/)

{% include links.html %}
