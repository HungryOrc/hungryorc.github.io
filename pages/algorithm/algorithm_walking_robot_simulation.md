---
title: "Walking Robot Simulation"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_walking_robot_simulation.html
folder: algorithm
toc: false
---

## Description: Easy
哦也

### Example
* Input: 
  * Output: 

## Solution：注意把握好4个方向，本题里的这些方向的概念 是符合小学数学二维坐标的概念，而非编程的惯例，即：向北即向上走是y+1而非x-1，向右走是x+1而非y+1

### Complexity
* Time: O(number of commands * avg length of each command)，比如转向的length为1，走5步的length为5
* Space: O(number of obstacles)，因为我们弄了个set

### Java
思路非常简单。代码看起来有点长
```java
class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        int[] curCoord = new int[2];
        int curDir = 0; // directions: 0: up, 1: right, 2: down, 3: left
        int maxDist = 0;
        
        Set<List<Integer>> obsSet = new HashSet<>();
        for (int[] obs : obstacles) {
            List<Integer> coord = new ArrayList<>();
            coord.add(obs[0]);
            coord.add(obs[1]);
            obsSet.add(coord);
        }
        
        for (int command : commands) {
            if (command == -2) { // turn left
                curDir = getNewDirection(curDir, -1);
            } else if (command == -1) { // turn right
                curDir = getNewDirection(curDir, 1);
            } else if (command >= 1 && command <= 9) {
                curCoord = move(curCoord, curDir, command, obsSet);
                maxDist = Math.max(maxDist, curCoord[0] * curCoord[0] + curCoord[1] * curCoord[1]);
            } // else do nothing
        }
        return maxDist;
    }
    
    private int getNewDirection(int originalDir, int turn) {
        int newDir = originalDir + turn;
        if (newDir == -1) newDir = 3;
        else if (newDir == 4) newDir = 0;
        return newDir;
    }
    
    private int[] move(int[] curCoord, int dir, int steps, Set<List<Integer>> obsSet) {
        int deltaX = 0, deltaY = 0;
        if (dir == 0) deltaY = 1; // up
        else if (dir == 1) deltaX = 1; // right
        else if (dir == 2) deltaY = -1; // down
        else if (dir == 3) deltaX = -1; // left
        
        int x = curCoord[0], y = curCoord[1];
        
        
        
        for (int i = 0; i < steps; i++) {
            int newX = x + deltaX, newY = y + deltaY;
            
            if (isObstacle(newX, newY, obsSet)) {
                return new int[]{x, y};
            }

            x = newX;
            y = newY;
        }
        return new int[]{x, y};
    }
    
    private boolean isObstacle(int x, int y, Set<List<Integer>> obsSet) {
        List<Integer> coord = new ArrayList<>();
        coord.add(x);
        coord.add(y);
        return obsSet.contains(coord);        
    }
}
```

## Reference
* [Walking Robot Simulation [LeetCode]](https://leetcode.com/problems/walking-robot-simulation/description/)

{% include links.html %}
