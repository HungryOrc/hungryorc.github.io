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
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces 

### Example
* Input: "(1+(4+ 5+2)-3)+ (6+8)"
  * Output: 23

## Solution
哦也

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
                i--; 
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
