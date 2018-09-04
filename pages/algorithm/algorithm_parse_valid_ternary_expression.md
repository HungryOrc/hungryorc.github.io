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
* The length of the given string is ≤ 10000.
* Each number will contain only one digit.
* The conditional expressions group right-to-left (as usual in most languages).
* The condition will always be either T or F. That is, the condition will never be a digit.
* The result of the expression will always evaluate to either a digit 0-9, T or F.

### Example
* Input: 3 dishes in all, from pole A to pole C, the intermediate pole is B.
  * Output: 
    ```
    1: A -> C
    2: A -> B
    1: C -> B
    3: A -> C
    1: B -> A
    2: B -> C
    1: A -> C
    ```

## Solution
```
Moving N dishes from A to C = Moving N-1 dishes from A to B, with the help of C +
                              Moving 1 dish from A to C +
			      Moving N-1 dishes from B to C, with the help of A
```

### Complexity
* Time: O(2^n)
  * T(n) = 2 * T(n-1) + 1, so you can get the answer...
* Space: O(n)

### Java
This solution is a bit different to the requirement of the question above, but the overall methodology is the same.

```java
public class Hanoi {
	
    public void hanoiMove(int dishNum, char from, char to, char inter) {
	if (dishNum == 1) {
	    System.out.println("1: " + from + " -> " + to);
	    return;
	}
		
	hanoiMove(dishNum - 1, from, inter, to);
		
	System.out.println(dishNum + ": " + from + " -> " + to);
		
	hanoiMove(dishNum - 1, inter, to, from);
    }
	
    public static void main(String[] args) {
	Hanoi hano = new Hanoi();	
	hano.hanoiMove(4, 'A', 'C', 'B');
    } 
}
```

## Reference
* [Tower of Hanoi - LintCode](https://lintcode.com/problem/tower-of-hanoi/description)

{% include links.html %}
