---
title: "Shortest Distance in an Array: between any 'A' and 'B'"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortestDistanceInArray_betweenAnyAAndB.html
folder: algorithm
toc: false
---

## Description
在一个char array 里，可能有若干个A和B。求最近的一对A和B的距离。如果A或B不存在，则返回 -1

### Example
* Input: [O,P,A,M,B,Z,B,A,S,S]
  * Output: 1

## Solution
从左到右过一遍，遇到A就看左边最近的B在哪，遇到B就看左边最近的A在哪

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int shortestDist(char[] chars) {
        if (chars == null || chars.length <= 1) {
            return -1;
        }
        
        int n = chars.length;
        int indexLatestA = -1;
        int indexLatestB = -1;
        int minDist = Integer.MAX_VALUE;
        
        for (int i = 0; i < n; i++) {
            char c = chars[i];
            if (c == 'A') {
                indexLatestA = i;
                if (indexLatestB != -1) {
                    minDist = Math.min(minDist, i - indexLatestA);
                }
            } else if (c == 'B') {
                indexLatestB = i;
                if (indexLatestA != -1) {
                    minDist = Math.min(minDist, i - indexLatestB);
                }
            }
        }
        
        if (minDist == Integer.MAX_VALUE) {
            return -1;
        }
        return minDist;
    }
}
```

## Reference
没找到这题

{% include links.html %}
