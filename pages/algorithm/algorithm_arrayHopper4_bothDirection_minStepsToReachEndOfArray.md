---
title: "Array Hopper IV: Can Go Both Directios, Return Min Steps to Reach the End of Array"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_arrayHopper4_bothDirection_minStepsToReachEndOfArray.html
folder: algorithm
toc: false
---

## Description
Given an array A of non-negative integers, you are initially positioned at an arbitrary index of the array. 
A[i] means the maximum jump distance from that position (you can **either jump left or right**). 
Determine the minimum jumps you need to reach the right end of the array. 

Return -1 if you can not reach the right end of the array. 这题的难度是 hard。

Assumptions
* The given array is not null and has length of at least 1.
* If the length of the given array is 1, then we define the result to be 0, even if array[0] = 0.
  * 数组最后一个元素一定可以达到最后一个元素，就算这个元素的值为0，它也达到了，因为它自己本身就是最后一个，不用走也自然达到

### Example
* Input: {1, 3, 1, 2, 2}, initial index = 2
  * Output: 2. Jump to index 1 then to the right end of array
* Input: {2, 4, 1, 0, 0, 0}, initial index = 2
  * Output: 3. Jump to index 1 then to the right end of array
* Input: {4, 0, 1, 0, 0}, initial index = 2
  * Output: -1. Cannot reach the end of array starting at initial index = 2, no matter how you go left/right with any number of steps

## Solution: 一种综合了DP和图遍历的思想。其实DP就是一种图遍历。对于这题来说，相当于一种“图上的最短路径”
我们搞一个 **从最后一个元素开始，向前“逐层BFS”** 的做法！

首先把能够一步到达n-1位置的indexes找出来，放到queue里。然后把这些indexes弹出来，再找能一步到达它们的indexes，再放到queue里去。以此类推。
注意去重。每搞这么一轮，step number 就得 +1。在这个过程里，一旦遇到题目给的 starting index，就返回当前的 step number

为了用 O(1) 的时间辨别出 哪些index能一步跳到一个给定的index，我们搞一个map。map的key是数组里的一个index，value是一个set，
set里装的是所有能一步到达这个index的indexes。这个map的填写就把题目给的数组walk through 一遍就填好了

### Complexity
* Time: O(n^2)，2层 for loop
* Space: O(n^2), HashMap

### Java
代码虽然看起来长，但逻辑并不复杂，看上面的思路说明
```java
public class Solution {
    public int minJump(int[] jumps, int index) {
        if (jumps == null || jumps.length == 0 ||
            index < 0 || index >= jumps.length) {
            return -1;
        }
      
        if (index == jumps.length - 1) {
            return 0;
        }
      
        int n = jumps.length;
      
        // <目的点index, {可以到达目的点的出发点index}>
        Map<Integer, HashSet<Integer>> reachable = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int radius = jumps[i];
            int leftEnd = Math.max(0, i - radius);
            int rightEnd = Math.min(n - 1, i + radius);
            
            for (int j = leftEnd; j <= rightEnd; j++) {
                HashSet<Integer> startPoints = 
                    reachable.getOrDefault(j, new HashSet<Integer>());
                startPoints.add(i);
                reachable.put(j, startPoints);
            }
        }
      
        // 找出所有可以一步到达 n-1 位置的 indexes，把它们放到queue里
        Queue<Integer> queue = new LinkedList<>();
        Set<Integer> used = new HashSet<>();
      
        Set<Integer> oneStepIndexes = reachable.get(n - 1);
        oneStepIndexes.remove(n - 1); // 要把n-1自己去掉！因为它不是一步到达！而是零步到达！
        for (int ind : oneStepIndexes) {
            queue.offer(ind);
            used.add(ind);
        }
      
        int stepNum = 1; // 几步能到n-1处
        while (!queue.isEmpty()) {
            int curSize = queue.size();
            
            for (int i = 0; i < curSize; i++) {
                int curIndex = queue.poll();
                
                if (curIndex == index) {
                    return stepNum;
                }
              
                Set<Integer> indexesThatCanReachCurIndexInOneStep =
                    reachable.get(curIndex);
                for (int ind : indexesThatCanReachCurIndexInOneStep) {
                    if (used.contains(ind)) {
                        continue;
                    }
                    queue.offer(ind);
                    used.add(ind);
                }
            }
            stepNum++;
        }
        return -1;
    }
}
```

## Reference
* [Array Hopper IV [LaiCode]](https://app.laicode.io/app/problem/91)

{% include links.html %}
