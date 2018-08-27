---
title: "Raindrop on Wood"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_raindrop_on_wood.html
folder: algorithm
toc: false
---

## Description
有一块木头长100cm。设木头的方向为x轴，木头的一端为0，另一端为100。雨滴可以打到任何的x坐标，包括小于0或大于100的坐标。雨滴的个数可以非常大。每个雨滴的宽度都是1cm。每个雨滴打湿的范围都是 [start, start + 1)，注意左边是闭区间，右边是开区间。

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
        double leadSpeedCurGrp = speeds[0];
    
        for (int i = 1; i < speeds.length; i++) {
            double curSpeed = speeds[i];
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
