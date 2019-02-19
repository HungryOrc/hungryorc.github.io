---
title: "Min Number of Rescue Boats"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_min_number_of_rescue_boats.html
folder: algorithm
toc: false
---

## Description
要从一个沉船/小岛上撤离所有的人。所有人的weight放到一个int数组里。所有救生艇的重量承载力是一样的。求最少需要多少条救生艇，才能把所有人都救走

Note:
* 默认参数都valid。包括所有人的weights都 > 0，船的 weight capacity 也 > 0，而且要大于等于最重的那个人的weight。

注意！这题和 cut wood，merge piles of stones，打气球 等可以用DP做的题目的重大区别在于，cut wood 等必须按照一定的顺序来安排元素，而
这题运人没有任何顺序，任何人都可以上任何船。

### Example
* Input: [2,2,3,5,4], limit of each boat is 5
  * Output: 4

## Solution：老老实实用permutation做

这类题不要随便用greedy做！绝大部分的greedy做法在数学上都是错误的！

### Complexity
* Time: O(n * n!)
* Space: O(n)

### Java
代码看起来不很短，但核心逻辑其实很短，只在一个函数里
```java
public class Solution {
    public int minNumOfRescueBoats(int[] weights, int capacity) {
        // ignore sanity checks
        int[] minNumBoats = new int[] {Integer.MAX_VALUE};
        
        bfs(weights, 0, capacity, minNumBoats);
        return minNumBoats[0];
    }
    
    // 核心逻辑都在这个函数里
    private void bfs(int[] weights, int index, int capacity, int[] minNumBoats) {
        // 完成了所有位的swap(或不swap)，形成了一个新的独特的数组
        if (index == weights.length) {
            minNumBoats[0] = Math.min(minNumBoats[0], 
                                      countMinRescueBoats(weights, capacity));
            return;
        }
        
        for (int i = index; i < weights.length; i++) {
            swap(weights, index, i);
            bfs(weights, index + 1, capacity, minNumBoats);
            swap(weights, index, i); // 复原
        }
    }
    
    private int countMinRescueBoats(int[] weights, int capacity) {
        int count = 0;
        int sum = 0;
        
        for (int wei : weights) {
            if (sum + wei > capacity) {
                count++;
                sum = wei;
                continue;
            }
            sum += wei;
        }
        
        if (sum > 0) { // 别疏漏了这种情况！
            return count + 1;
        }
        return count;
    }
    
    private void swap(int[] weights, int i, int j) {
        int tmp = weights[i];
        weights[i] = weights[j];
        weights[j] = tmp;
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {
        int[] weights = {2, 2, 3, 5, 4};
        int capacity = 5;
        
        Solution solu = new Solution();
        int result = solu.minNumOfRescueBoats(weights, capacity);
        System.out.println(result); // 4
    }
}
```

## Reference
没找到这题

{% include links.html %}
