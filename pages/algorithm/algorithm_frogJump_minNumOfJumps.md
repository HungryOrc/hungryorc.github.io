---
title: "Frog Jump: Min Number of Jumps to Cross the River"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_frogJump_minNumOfJumps.html
folder: algorithm
toc: false
---

## Description
看LeetCode上Frog Jump 那题，那一题只是问是否能跳过河，这一题是网上没有的，是要求最少多少步能跳过河。
如果无法跳过河，则返回 Integer.MAX_VALUE。
跳过河的标准是要正好跳到最后一个石头上。

A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. 
The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, 
determine if the frog is able to cross the river by landing on the last stone. 
Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was k units, then its next jump must be either k - 1, k, or k + 1 units. 
Note that the frog can only jump in the forward direction.

Note
* The number of stones is ≥ 2 and is < 1100.
* Each stone's position will be a non-negative integer < 2^31.
* The first stone's position is always 0.

### Example
* Input: [0,1,3,5,6,8,12,17]，注意数组里的每个数字表示石头的坐标，从0开始编index，即数组里的0表示小河里的第1个位置有石头，数组里的3表示小河里的第4个位置有石头
  * Output: 6
  * The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.
* Input: [0,1,2,3,4,8,9,11]
  * Output: Integer.MAX_VALUE，无法跳到河的尽头，因为 the gap between the 5th and 6th stone is too large

## Solution: 我自己的 二维DP做法 <=== 对么 ？？？？
* int dp[i][j] 表示 **离开** stones数组里index为i的那个石头（不是指石头的position值为i），**离开的速度** 正好为j，最少多少步可以到达这个状态
  * 特别注意！这里的速度是离开这个石头时的速度！不是到达这个石头时的速度！这个区别很重要！如果搞混了，题目一定会做错
  * 这个dp矩阵里每个元素的初始值我们都填为 -1，如果最终都无法到达这个状态，也是 -1
* max possible speed 和 river length 的关系：按这题的题意，每次速度变化可以是不变，+1 或 -1，那么最大的速度就是每次都 +1，但当然不可能无限加速下去，加速到 river length 的时候就到头了，那么有：1 + 2 + 3 + 4 + ... + (maxSpeed-1) + maxSpeed >= riverLen，所以 maxSpeed * (maxSpeed + 1) / 2 >= riverLen，所以 maxSpeed 约等于 sqrt(riverLen * 2)

### Complexity
* Time: O(n * maxSpeed)，其中n是题目给的数组的长度，不是小河的长度；maxSpeed是小河的长度的平方根，具体解释见前面
* Space: O(n * maxSpeed * 3) = O(n * maxSpeed)，其中3是指三种速度变化方式

### Java
```java
class Solution {
    public int minJumps(int[] stones) {
        if (stones == null || stones.length == 0 || stones[0] != 0) {
            return -1; // 数据错误
        }
        
        int n = stones.length;
        if (n == 1) return 0; // 不用跳了
        
        int riverLen = stones[n - 1]; // 注意riverLen和n的区别
        int maxSpeed = (int)(Math.sqrt(riverLen * 2)); // max possible speed
        
        int[][] dp = new int[n][maxSpeed + 1];
        // 初始化
        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= maxSpeed; j++) {
                dp[i][j] = -1;
            }
        }
        
        // <stone position, stone index in the stnes array>
        Map<Integer, Integer> stoneMap = new HashMap<>();
        for (int i = 0; i < n; i++) {
            stoneMap.put(stones[i], i);
        }
        
        // base case
        dp[0][1] = 0; // the first position and first speed，初始不用跳就是这个状态，所以0
        
        for (int i = 1; i < n; i++) { // each stone in the river
            int curPos = stones[i]; // current position (must be a stone)
            
            for (int speed = 1; speed <= maxSpeed; speed++) { // each possible speed
                
                for (int diff = -1; diff <= 1; diff++) { // 3 possible prev speeds
                    int prevSpeed = speed + diff;
                    if (prevSpeed <= 0 || prevSpeed > maxSpeed) {
                        continue;
                    }
                    
                    // 注意！这里应该 减prevSpeed！不是 减curSpeed！
                    int prevPos = curPos - prevSpeed; 
                    if (prevPos <= 0) {
                        continue;
                    }
                    int prevStoneIndex = stoneMap.getOrDefault(prevPos, -1);
                    if (prevStoneIndex == -1) {
                        continue;
                    }
                    
                    // 别忘了判断 假设的之前的这个状态 是否可达
                    if (dp[prevStoneIndex][prevSpeed] ！= -1) { 
                        dp[i][speed] = Math.min(dp[i][speed], 
                                                dp[prevStoneIndex][prevSpeed] + 1);
                        break; // we are done with dp[i][speed] now
                    }
                }
            }
        }
        
        int minSteps = Integer.MAX_VALUE;
        for (int num : dp[n - 1]) {
            if (num != -1) { // 别忘了查这一条
                minSteps = Math.min(minSteps, num);
            }
        }
        return minSteps;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
