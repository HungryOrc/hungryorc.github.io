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
n 个车依次出发，行驶于一条无限长的单车道上，行驶方向都相同。各个车都是匀速行驶，它们的速度都记录在数组 `double[] speeds` 里。那么经过无限长的时间以后，所有的车都会被分组，因为后面的车永远不可能超过前面的车。求最后分为了几个组，每个组里有几辆车。

可以看出，在这题的条件下，相邻的两辆车之间的出发间隔时间的大小是无所谓的。

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
