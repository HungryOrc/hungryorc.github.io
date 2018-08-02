---
title: "Degree of Array"
tags: [algorithm, subarray]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_degree_of_array.html
folder: algorithm
toc: false
---

## Description

Tower of Hanoi problem, is a well-known problem. On the A, B, C three pillars, there are n disks of different sizes (radii 1-n), they are stacked in a start on A, your goal is to a minimum number of legal steps to move all the plates move from A to C tower tower.
Each step in the rules of the game are as follows:

Each step is only allowed to move a plate (from the top of one pillars to the top of another pillars)
The process of moving, you must ensure that a large dish is not at the top of the small plates (small can be placed on top of a large, below the maximum plate size can not have any other dish)

## Example

* Input: `3, 'A', 'C', 'B'` 
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

目标子数组的首元素一定是 nums 中出现最多的元素（或之一，设它为 a）在整个数组里的第一次出现。
目标子数组的尾元素一定是同样的这个 a 在整个数组里的最后一次出现。
所以问题转化为：求nums中出现次数最多的某个元素第一次出现，到最后一次出现的子数组的最小长度。

所以只需 walk through 数组 nums 一次，在此过程中用 2 个 map：一个记录当前元素值到目前为止出现的次数；另一个记录当前元素值出现的首位置，用于计算子数组长度。过程中要不断判断：
* 是否需要更新 所有元素值出现的最大次数 maxCount
* 如果当前元素值的出现次数 curCount == maxCount，可能需要更新 最短子数组长度 minLen。如果 curCount > maxCount，则必须更新 minLen。

### Complexity

Time: O(n), Space: O(n)

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
