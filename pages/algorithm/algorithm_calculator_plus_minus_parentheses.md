---
title: "Calculator with Plus, Minus and Parentheses"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_calculator_plus_minus_parentheses.html
folder: algorithm
toc: false
---

## Description
Implement a basic calculator to evaluate a simple expression string. 我们默认这个expression是valid的。

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces

### Example
* Input: "(1+(4+ 5+2)-3)+ (6+8)"
  * Output: 23
* Input: "(7)- (0)+ (5)"
  * Output: 12
* Input: " (1)"
  * Output: 1
  
## Solution
几个重点：
* string默认是valid的
* 数字和运算符分别放在两个stack里
* 数字可能有多位。但没有负数
* 这题里面只有+和-，所以对于连在一起的+和-（连在一起的意思是相邻而且没有任何左或右括号隔开），从左到右算就对了（比如 7-6+1，先算后面的6+1就错了）
  * 那么，看到一个+或者-的时候，就看它前面还有没有连在一起的+或-，有的话就**先处理前面那个！**后面这个+或-放到运算符的stack里去
  * 算出来的阶段性结果的数字要放到数字的stack里去
* string里可能有多对括号，可能有嵌套括号。但是对于每一对括号来说，走到)的时候，按照前面的处理运算符的方式，那么一对( )中间此时最多有1个+或-，
最少则可能一个+或-都没有，只有一个数字（这种可能性别忘了）
* string的最右边一个char走完以后，不会再存在任何左或者右括号。但有可能存在一个+或者-（这种情况下一定还存在2个数字），或者只存在一个数字。按照上述
的方法，不可能存在超过一个+或者-。

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    
    public int calculate(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        Stack<Integer> nums = new Stack<>();
        Stack<Character> optrs = new Stack<>();
        
        char[] chars = s.toCharArray();
        int len = chars.length;
        
        for (int i = 0; i < len; i++) {
            char c = chars[i];

            if (Character.isDigit(c)) {
                int num = 0;
                while (i < len && Character.isDigit(chars[i])) {
                    num = num * 10 + (chars[i] - '0');
                    i++;
                }
                i--; // don't forget this
                nums.push(num);
            } else if (c == '+' || c == '-') {
                // this "if" means there is a "+" or "-"" in the optrs stack, before the current optr namely c
                if (!optrs.isEmpty() && optrs.peek() != '(') { 
                    int num = doCalc(nums.pop(), optrs.pop(), nums.pop());
                    nums.push(num);
                }
                optrs.push(c); // after dealing with the prev optr, we push the current optr into the stack
            } else if (c == '(') {
                optrs.push(c);
            } else if (c == ')') {
                // this "if" means there is a "+" or "-"" in the pair of '(' and ')',
                // and by the current solution, there can be at most only one '+' or '-' left between 
                // the '(' and the ')' when it reaches the ')',
                // there can also be zero '+' or '-', like "(7)", this does look good but is a valid case!
                if (!optrs.isEmpty() && optrs.peek() != '(') {
                    nums.push(doCalc(nums.pop(), optrs.pop(), nums.pop()));
                }
                optrs.pop(); // remove the '('
            } // else c == ' ', we don't care
        }
        
        // by the current solution, at this very last moment, there can be at most one '+' or '-' left
        // that is not processed yet. Because there is only '+' and '-' in the original expression, and we 
        // should have dealt with most of them one by one from left to right.
        // If there were '*' or '/' in the original expression, then we might have more than one operators
        // at the very last moment.
        if (!optrs.isEmpty()) {
            nums.push(doCalc(nums.pop(), optrs.pop(), nums.pop()));
        }
        
        return nums.pop();
    }
    
    private int doCalc(int num2, char optr, int num1) {
        if (optr == '+') {
            return num1 + num2;
        } else { // optr == '-'
            return num1 - num2;
        }
    }
}
```

## Reference
* [Basic Calculator [LeetCode]](https://leetcode.com/problems/basic-calculator/description/)

{% include links.html %}
