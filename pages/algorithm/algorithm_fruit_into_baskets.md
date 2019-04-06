---
title: "Fruit Into Baskets"
tags: [algorithm, sliding_window]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_fruit_into_baskets.html
folder: algorithm
toc: false
---

## Description
In a row of trees, the i-th tree produces fruit with type tree[i].

You **can start at any tree**, then repeatedly perform the following steps:
1. Add one piece of fruit from this tree to your baskets.  If you cannot, stop.
2. Move to the next tree to the right of the current tree.  If there is no tree to the right, stop.

Note that you do not have any choice after the initial choice of starting tree: you must perform step 1, then step 2, then back to step 1, then step 2, and so on until you stop.

You have 2 baskets, and each basket can carry any quantity of fruit, but you want each basket to only carry one type of fruit each.

What is the MAX total amount of fruits you can collect with this procedure?
* 1 <= tree.length <= 40000
* 0 <= tree[i] < tree.length

### Example
* Input: 
  * Output: 
Example 1:

Input: [1,2,1]
Output: 3
Explanation: We can collect [1,2,1].
Example 2:

Input: [0,1,2,2]
Output: 3
Explanation: We can collect [1,2,2].
If we started at the first tree, we would only collect [0, 1].
Example 3:

Input: [1,2,3,2,2]
Output: 4
Explanation: We can collect [2,3,2,2].
If we started at the first tree, we would only collect [1, 2].
Example 4:

Input: [3,3,3,1,2,1,1,2,3,3,4]
Output: 5
Explanation: We can collect [1,2,1,1,2].
If we started at the first tree or the eighth tree, we would only collect 4 fruits.

## Solution 1： 我的很直觉的方法，sliding window, 速度 前45%。后面有改进后更快的方法
容易看出，这题就是 max length of sliding window which contains no more than 2 distinct values

### Complexity
* Time: O(n)
* Space: O(1), map size 一直是 2

### Java
```java
class Solution {
    public int totalFruit(int[] tree) {
        if (tree == null || tree.length == 0) {
            return 0;
        }
        
        int n = tree.length;
        if (n <= 2) {
            return n;
        }
        
        // <fruit type, count>
        Map<Integer, Integer> map = new HashMap<>();
        int fast = 0, slow = 0;
        int fruitCount = 0;
        int maxFruitCount = 0;
        
        while (fast < n) {
            int fastVal = tree[fast];
            int slowVal = tree[slow];
            Integer fastCount = map.get(fastVal);
            
            if (fastCount != null) {
                map.put(fastVal, fastCount + 1);
                fast++;
                fruitCount++;
                maxFruitCount = Math.max(maxFruitCount, fruitCount);
            } else if (map.size() < 2) {
                map.put(fastVal, 1);
                fast++;
                fruitCount++;
                maxFruitCount = Math.max(maxFruitCount, fruitCount);
            } else { // map.size() == 2
                int slowCount = map.get(slowVal);
                if (slowCount == 1) {
                    map.remove(slowVal);
                    slow++;
                    fruitCount--;
                } else {
                    map.put(slowVal, slowCount - 1);
                    slow++;
                    fruitCount--;
                }
            }
        }
        return maxFruitCount;
    }
}
```

## Reference
* [Fruit into Baskets [LeetCode]](https://leetcode.com/problems/fruit-into-baskets/description/)

{% include links.html %}
