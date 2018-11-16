---
title: "Burst Balloons"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_burst_balloons.html
folder: algorithm
toc: false
---

## Description
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. 
You are asked to burst all the balloons. If the you burst balloon i you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. 
After the burst, the i - 1 and i + 1 balloons becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

* You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them. 所以如果打到 index = 0 或者 
n - 1 的气球，就用这个规则计分
* 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100。这里 500 这个数字其实就是一个暗示！暗示要用 DP 来做。因为 500 量级用DFS做的话过于巨大，会冒

### Example
* Input: [3,1,5,8]
  * Output: 16
  ```
   nums  =  [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  -->  []
            burst 1       burst 5      burst 3     burst 8     all balloons gone
  coins  =   3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   =  167
  ```

## Solution: 中心开花 DP
这一题的难点在于，如何处理爆掉某一个气球以后，它两边的气球变得相邻。那么原来相距好远的气球也可能相邻。这样的不断缩并，用DP做是较好的方法。

用 **中心开花** 的DP方法来做。
* 最后所有的气球都是要爆的。我们可以从编号为i到j的一个区间的所有气球被爆掉这样的案例入手
* 在i到j这一段上，最后一枪打爆的可能是从i到j的任何一个气球，i和j都是inclusive。那么从i到j全部被打爆的最高的总得分，就是这j-1+1种最后一枪的情况里，
得分最高的那一个。
  * 这最后一枪不论打在哪里，它都会把整段分出左右两个分段，那么在分段上也是同样的方法论。如果最后一枪打在i或j，代码处理起来也是一样的。见下面代码

步骤：
* 

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [Burst Balloons [LeetCode]](https://leetcode.com/problems/burst-balloons/description/)

{% include links.html %}
