---
title: "Car Grouping by Speed"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_car_grouping_by_speed.html
folder: algorithm
toc: false
---

## Description

Tower of Hanoi problem, is a well-known problem. On the A, B, C three pillars, there are n disks of different sizes (radii 1-n), they are stacked in a start on A, your goal is to a minimum number of legal steps to move all the plates move from A to C tower tower.
Each step in the rules of the game are as follows:

Each step is only allowed to move a plate (from the top of one pillars to the top of another pillars)
The process of moving, you must ensure that a large dish is not at the top of the small plates (small can be placed on top of a large, below the maximum plate size can not have any other dish)

## Example

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
