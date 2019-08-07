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

## Solution 1： 我的直觉方法，sliding window, 速度 前45%。后面有改进后更快的方法
容易看出，这题就是 **max length of sliding window which contains NO MORE THAN 2 DISTINCT VALUES**

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
                } else {
                    map.put(slowVal, slowCount - 1);
                }
                slow++;
                fruitCount--;
            }
        }
        return maxFruitCount;
    }
}
```

## Solution 2：我的方法，还是sliding window，但要记录两个元素最后(最右边)出现的位置(即index)，而且要记录这两个元素里哪个出现的最后一次更靠左，而且这个靠左和不靠左会不断交替 或者被第三者改变。速度 前10%

这个思路很符合直觉，不管程序实现，在脑子里用脑子想清楚过程并不难。实现代码的时候要细心一点，把各个情况都掰开掰碎，逐个写清楚就ok了。水到渠成。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int totalFruit(int[] tree) {
        int n = tree.length;
        
        int former = tree[0];
        int lastIndexOfFormer = 0;
        int latter = Integer.MAX_VALUE;
        int lastIndexOfLatter = -1;
        
        int startOfCurSubarray = 0;
        
        int maxLen = 1;
        
        for (int i = 1; i < n; i++) {
            int cur = tree[i];
            
            // 如果第二个数还没出现，即 整个数组到现在为止 都只出现过一个数
            if (latter == Integer.MAX_VALUE) { 
                if (cur == former) {
                    lastIndexOfFormer = i;
                    maxLen = i + 1;
                } else {
                    latter = cur;
                    lastIndexOfLatter = i;
                    maxLen = i + 1;
                }
                continue;
            }
            
            // 下面都是数组里迄今为止 已经出现过至少2种不同的值 的情况了
            
            if (cur == latter) {
                lastIndexOfLatter = i;
                maxLen = Math.max(maxLen, i - startOfCurSubarray + 1);
            }
            else if (cur == former) {
                former = latter;
                latter = cur;
                
                lastIndexOfLatter = i;
                lastIndexOfFormer = i - 1;
                
                maxLen = Math.max(maxLen, i - startOfCurSubarray + 1);
            }
            else { // cur 是不同于当前篮子里的2种水果的 第3种水果
                former = latter;
                latter = cur;
                
                startOfCurSubarray = lastIndexOfFormer + 1;
                
                lastIndexOfFormer = i - 1;
                lastIndexOfLatter = i;
                
                maxLen = Math.max(maxLen, i - startOfCurSubarray + 1);
            }
        }
        return maxLen;
    }
}
```

## Reference
* [Fruit into Baskets [LeetCode]](https://leetcode.com/problems/fruit-into-baskets/description/)

{% include links.html %}
