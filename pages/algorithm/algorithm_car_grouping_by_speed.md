---
title: "Car Grouping by Speed"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_car_grouping_by_speed.html
folder: algorithm
toc: false
---

## Description
n 个车依次出发，行驶于一条无限长的单车道上，行驶方向都相同。各个车都是匀速行驶，它们的速度都记录在数组 `double[] speeds` 里。那么经过无限长的时间以后，所有的车都会被分组，因为后面的车永远不可能超过前面的车。求最后分为了几个组，每个组里有几辆车。

可以看出，在这题的条件下，相邻的两辆车之间的出发间隔时间的大小是无所谓的。

## Example

* Input: 车速依次为：[2.5, 4, 1, 3.1, 1, 0.5]
  * Output: [2, 2, 1, 1]，因为最后会被分为4组，第一组是前两辆车，第二组是第三第四两辆车，第三组是第五辆车，第四组是第六辆车

## Solution
要点：
* 相邻两个车，后面的车速如果高于前面的，那么后面车的车速最终一定会等于前车车速。它们会组成一个group，当然这个group里可能还会有别的车。
  * 所以最终所有的车会组成多个这样的group，每个group有一个领头的车，一个group内的所有车的车速都等于领头车的车速。
* 如果全部的groups都稳定以后，某一个领头车的速度是 v，后面若干个车以后的某个车的速度小于 v，那么这两个车之间的距离会越来越大。如果后面这个车的速度等于 v，那么这两个车之间永远不会相遇，这个速度相等又不会相遇的情况容易被遗漏，要注意。

所以，把这个数组从头到尾遍历一遍就行了。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {

    public List<Integer> grouping(double[] speeds) {
        if (speeds == null || speeds.length == 0) {
            return null;
        }

        List<Integer> groupSizes = new ArrayList<>();

        int countCurGrp = 1;
        int leadSpeedCurGrp = speeds[0];
    
        for (int i = 1; i < speeds.length; i++) {
            int curSpeed = speeds[i];
            if (curSpeed > leadSpeedCurGrp) {
                countCurGrp ++;
            } else { // if (curSpeed <= leadSpeedCurGrp)
                groupSizes.add(countCurGrp);
                countCurGrp = 1;
                leadSpeedCurGrp = curSpeed;
            }
        }

        groupSizes.add(countCurGrp); // for the last group

        return groupSizes;
    }
}
```

## Reference


{% include links.html %}
