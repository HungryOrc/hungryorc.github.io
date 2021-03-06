---
title: "Tower of Hanoi"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_tower_of_hanoi.html
folder: algorithm
toc: false
---

## Description
Tower of Hanoi problem, is a well-known problem. On the A, B, C three pillars, there are n disks of different sizes (radii 1-n), they are stacked in a start on A, your goal is to a minimum number of legal steps to move all the plates move from A to C tower tower.
Each step in the rules of the game are as follows:

Each step is only allowed to move a plate (from the top of one pillars to the top of another pillars)
The process of moving, you must ensure that a large dish is not at the top of the small plates (small can be placed on top of a large, below the maximum plate size can not have any other dish)

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
class Hanoi {	
    public void hanoiMove(int dishNum, char from, char to, char intermediate) {
	if (dishNum == 1) {
	    System.out.println("Dish 1: " + from + " -> " + to);
	    return;
	}
	
	// step 1: 把从最小到倒数第二大的dishes 从初始柱子 挪到中间的柱子上去
	hanoiMove(dishNum - 1, from, intermediate, to);
	
	// step 2: 把最大的dish 从初始柱子 挪到最后的柱子上去
	System.out.println("Dish " + dishNum + ": " + from + " -> " + to);
	
	// step 3：把从最小到倒数第二大的dishes 从中间的柱子 挪到最后的柱子上去
	hanoiMove(dishNum - 1, intermediate, to, from);
    }
	
    public static void main(String[] args) {
	Hanoi hano = new Hanoi();	
	hano.hanoiMove(4, 'A', 'C', 'B');
    } 
}
```

## Reference
* [Tower of Hanoi [LintCode]](https://lintcode.com/problem/tower-of-hanoi/description)

{% include links.html %}
