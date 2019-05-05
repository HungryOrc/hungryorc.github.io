---
title: "Water and Jug"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_water_and_jug_来回倒水.html
folder: algorithm
toc: false
---

## Description
You are given two jugs with capacities x and y litres. There is an infinite amount of water supply available. You need to determine whether it is possible to measure exactly z litres using these two jugs.

If z liters of water is measurable, you must have z liters of water contained within **one or BOTH** buckets by the end.

Operations allowed:
* Fill any of the jugs completely with water.
* Empty any of the jugs.
* Pour water from one jug into another till the other jug is completely full or the first jug itself is empty.

### Example
* Input: x = 3, y = 5, z = 4
  * Output: True
* Input: x = 2, y = 6, z = 5
  * Output: False

## Solution 1：我自己的方法，思路很清晰，但速度慢。回头要看下LC答案的解法！
如果一开始2个瓶子的容量是3升和5升，那么很容易得到5-3=2升
* 这个2升如果是放在5里，则
  * 3还可以满上，所以可以得到3+2
  * 3可以满上以后往5里倒，如果倒不光，则3里剩下的量也是可算出来的（虽然此时会倒光）
* 这个2升如果是放在3里(如果放得下)，则
  * 5还可以满上，就得到5+2
  * 5满上以后往3倒，5里剩下的就是 5 - (3 - 2)
* 依次类推！新造出来的数，如果能放入 5 或 5以及3，则都可以用来产生新的数！把它们放到queue里去待产！

### Complexity
* Time: O(x + y) <=== ？？？
* Space: O(x + y) <=== ？？？

### Java
```java
class Solution {
    public boolean canMeasureWater(int x, int y, int z) {
        if (x < 0 || y < 0 || z < 0) {
            return false;
        }
        if (z == x || z == y || z == 0 || z == x + y) { // 注意这些特例，挺tricky的
            return true;
        }
        
        int smaller = x;
        int bigger = y;
        if (x > y) {
            smaller = y;
            bigger = x;
        }
        
        Set<Integer> visited = new HashSet<>();
        visited.add(smaller);
        visited.add(bigger);
        visited.add(bigger - smaller);
        
        Queue<Integer> toCheck = new LinkedList<>();
        toCheck.offer(bigger - smaller);
        
        while (!toCheck.isEmpty()) {
            int curNum = toCheck.poll();

            if (curNum == z) {
                return true;
            }
            
            // 下面考查，只再倒水一步，能得到哪些水量
            if (curNum > bigger) { // 此curNum无处安放
                // do nothing
            } else if (curNum > smaller) { // 此curNum必然是装在bigger jar里的
                process(curNum + smaller, toCheck, visited);
                process(curNum - smaller, toCheck, visited);
                process(smaller - (bigger - curNum), toCheck, visited);
            } else { // curNum < smaller，此curNum可以装在smaller 或 bigger jar里
                // 如果装在bigger jar里
                process(curNum + smaller, toCheck, visited);
                process(smaller - (bigger - curNum), toCheck, visited); 
                // 如果装在smaller jar里
                process(curNum + bigger, toCheck, visited);
                process(bigger - (smaller - curNum), toCheck, visited);                 
            }
        }
        return false;
    }
    
    private void process(int num, Queue<Integer> toCheck, Set<Integer> visited) {
        if (num <= 0) {
            return;
        }
        
        if (visited.contains(num)) {
            return;
        }
        visited.add(num);
        
        toCheck.offer(num);
    }
}
```

## Reference
* [Water and Jug Problem [LeetCode]](https://leetcode.com/problems/water-and-jug-problem/description/)

{% include links.html %}
