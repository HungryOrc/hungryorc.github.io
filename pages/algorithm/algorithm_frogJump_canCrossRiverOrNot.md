---
title: "Frog Jump: Can Cross River Or Not"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_frogJump_canCrossRiverOrNot.html
folder: algorithm
toc: false
---

## Description
A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was k units, then its next jump must be either k - 1, k, or k + 1 units. Note that the frog can only jump in the forward direction.

Note
* The number of stones is ≥ 2 and is < 1100.
* Each stone's position will be a non-negative integer < 2^31.
* The first stone's position is always 0.

### Example
* Input: [0,1,3,5,6,8,12,17]，注意数组里的每个数字表示石头的坐标，从0开始编index，即数组里的0表示小河里的第1个位置有石头，数组里的3表示小河里的第4个位置有石头
  * Output: True
  * The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.
* Input: [0,1,2,3,4,8,9,11]
  * Output: False
  * There is no way to jump to the last stone as the gap between the 5th and 6th stone is too large.

## Solution 1: 我自己的 二维DP做法，实测速度很慢，但自我感觉挺好
* boolean dp[i][j] 表示 **离开** stones数组里index为i的那个石头（不是指石头的position值为i），**离开的速度** 正好为j，是否有可能实现这种状态
  * 特别注意！这里的速度是离开这个石头时的速度！不是到达这个石头时的速度！这个区别很重要！如果搞混了，题目一定会做错
* max possible speed 和 river length 的关系：按这题的题意，每次速度变化可以是不变，+1 或 -1，那么最大的速度就是每次都 +1，但当然不可能无限加速下去，加速到 river length 的时候就到头了，那么有：1 + 2 + 3 + 4 + ... + (maxSpeed-1) + maxSpeed >= riverLen，所以 maxSpeed * (maxSpeed + 1) / 2 >= riverLen，所以 maxSpeed 约等于 sqrt(riverLen * 2)

### Complexity
* Time: O(n * maxSpeed)，其中n是题目给的数组的长度，不是小河的长度；maxSpeed是小河的长度的平方根，具体解释见前面
* Space: O(n * maxSpeed * 3) = O(n * maxSpeed)，其中3是指三种速度变化方式

### Java
```java
class Solution {
    public boolean canCross(int[] stones) {
        if (stones == null || stones.length == 0 || stones[0] != 0) {
            return false;
        }
        
        int n = stones.length;
        if (n == 1) return true;
        
        int riverLen = stones[n - 1] + 1; // 注意riverLen和n的区别
        int maxSpeed = (int)(Math.sqrt(riverLen * 2)); // max possible speed
        
        boolean[][] dp = new boolean[n][maxSpeed + 1];
        
        // <stone position, stone index in the stnes array>
        Map<Integer, Integer> stoneMap = new HashMap<>();
        for (int i = 0; i < n; i++) {
            stoneMap.put(stones[i], i);
        }
        
        // base case
        dp[0][1] = true; // the first position and first speed
        
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
                    if (prevPos < 0) {
                        continue;
                    }
                    int prevStoneIndex = stoneMap.getOrDefault(prevPos, -1);
                    if (prevStoneIndex == -1) {
                        continue;
                    }
                    
                    if (dp[prevStoneIndex][prevSpeed]) {
                        dp[i][speed] = true;
                        
                        if (i == n - 1) {
                            return true;
                        }
                        
                        break; // we are done with dp[i][speed] now
                    }
                }
            }
        }
        return false;
    }
}
```

## Solution 2: BFS，比上面的DP还慢。DFS的代码和BFS基本一样，只是把queue改为stack就行，速度也一样很慢

### Complexity
* Time: O(3^n)，其中n是题目给的数组的长度
* Space: O(n), queue size

### Java
```java
// helper class
class State {
    int position;
    int speed; // 离开此position时的速度
    public State(int p, int s) {
        this.position = p;
        this.speed = s;
    }
}

public class Solution {
    public boolean canCross(int[] stones) {
        if (stones == null || stones.length == 0 || stones[0] != 0) {
            return false;
        }
        
        int n = stones.length;
        if (n == 1) return true;
        
        int riverLen = stones[n - 1];
        
        Set<Integer> validPositions = new HashSet<>();
        for (int stone : stones) {
            validPositions.add(stone);
        }
        
        Queue<State> queue = new LinkedList<>();
        State init = new State(0, 1); // initial state: position 0, speed 1
        queue.offer(init);
        
        while (!queue.isEmpty()) {
            State cur = queue.poll();
            int curPos = cur.position;
            int curSpeed = cur.speed;
            
            for (int diff = -1; diff <= 1; diff++) {
                int newSpeed = curSpeed + diff;
                if (newSpeed <= 0) {
                    continue;
                }
                
                // 注意！这里是 + curSpeed！不是 + newSpeed！
                // 因为 State 里的speed都是 离开速度！不是 到达速度！
                int newPos = curPos + curSpeed;
                if (newPos <= 0 || newPos > riverLen || !validPositions.contains(newPos)) {
                    continue;
                }
                if (newPos == riverLen) {
                    return true;
                }
                
                queue.offer(new State(newPos, newSpeed));
            }
        }
        return false;
    }
}
```

## Reference
* [Frog Jump [LeetCode]](https://leetcode.com/problems/frog-jump/description/)

{% include links.html %}
