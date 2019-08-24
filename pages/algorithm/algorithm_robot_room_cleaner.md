---
title: "Robot Room Cleaner"
tags: [algorithm, dfs]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_robot_room_cleaner.html
folder: algorithm
toc: false
---

## Description: 这题是 Hard 类，但也不是那么难
Given a robot cleaner in a room modeled as a grid.

Each cell in the grid can be empty or blocked.

The robot cleaner with 4 given APIs can move forward, turn left or turn right. Each turn it made is 90 degrees.

When it tries to move into a blocked cell, its bumper sensor detects the obstacle and it stays on the current cell.

Design an algorithm to clean the entire room using only the 4 given APIs shown below:
```java
interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
```

Note:
* The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the mentioned 4 APIs, without knowing the room layout and the initial robot's position.
* The robot's initial position will always be in an accessible cell.
* The initial direction of the robot will be facing up.
* All accessible cells are connected, which means the all cells marked as 1 will be accessible by the robot.
* Assume all four edges of the grid are all surrounded by wall.

### Example
* Input: 
  ```
  room = [
    [1,1,1,1,1,0,1,1],
    [1,1,1,1,1,0,1,1],
    [1,0,1,1,1,1,1,1],
    [0,0,0,1,0,0,0,0],
    [1,1,1,1,1,1,1,1]
  ],
  row = 1,
  col = 3
  ```
  * Output: All grids in the room are marked by either 0 or 1. 0 means the cell is blocked, while 1 means the cell is accessible. The robot initially starts at the position of row=1, col=3. From the top left corner, its position is one row below and three columns right.

## Solution 1: DFS，速度较慢，回头我看下有没有更快的方法
Ref: https://leetcode.com/problems/robot-room-cleaner/discuss/139057/Very-easy-to-understand-Java-solution

### Complexity
* Time: O(n * m) <==== 对么 ？？？？
* Space: O(n * m) <==== 对么 ？？？？

### Java
```java

class Solution {
    public void cleanRoom(Robot robot) {
        Set<List<Integer>> visited = new HashSet<>();
        
        // robot starts at coordinate (0, 0)
        // actually in this question the starting point does not matter
        // 
        // The robot's first direction is upwards, I set the directions as:
        // 1: up, 2: left, 3: down, 4: right
        dfs(robot, visited, 0, 0, 1);
    }
    
    private void dfs(Robot robot, Set<List<Integer>> visited, int x, int y, int direction) {
        List<Integer> curCoord = new ArrayList<>();
        curCoord.add(x);
        curCoord.add(y);
        
        if (visited.contains(curCoord)) {
            return;
        }
        visited.add(curCoord);
        robot.clean();

        for (int numOfTurns = 0; numOfTurns < 4; numOfTurns++) {
            if (robot.move()) {
                int nextX = x;
                int nextY = y;
                
                if (direction == 1) {
                    nextX = x - 1;
                } else if (direction == 2) {
                    nextY = y - 1;
                } else if (direction == 3) {
                    nextX = x + 1;
                } else {
                    nextY = y + 1;
                }
                
                // 到了下一个位置的时候，一开始，方向和当前方向是一致的！这是一个不易想到的地方，
                // 也是很重要的地方，没有这个就不算是模拟机器人的行为，而是凭空的移动。
                // 机器人的这个方向的（暂时）保持，是因为比如我们向北从(x,y)走到(x-1,y)的时候，机器人
                // 刚到(x-1,y)的时候，它的脸必然还是向北的。那么它就要以向北为初始条件，转动四次，走四个方向。
                dfs(robot, visited, nextX, nextY, direction);
            
                // 从 (nextX, nextY) 坐标 回到 (x, y) 坐标
                robot.turnRight();
                robot.turnRight();
                robot.move();
                robot.turnRight();
                robot.turnRight();
            }
            
            // 别忘了这里要真的向左转一次（转4次就是试过了4个方向），
            // 不是光把direction加加就行了的，因为这是机器人的模拟移动，不是空虚的数学的移动
            robot.turnLeft();
            direction++;
            if (direction == 4) { direction = 0; }
        }
    }
}
```

## Reference
* [Robot Room Cleaner [LeetCode]](https://leetcode.com/problems/robot-room-cleaner/description/)

{% include links.html %}
