---
title: "Find Leftmost Right Interval"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_leftmost_right_interval.html
folder: algorithm
toc: false
---

## Description
Given a set of intervals, for each of the interval i, 
check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, 
which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, 
which means that the interval j has the minimum start point to build the "right" relationship for interval i. 
If the interval j doesn't exist, store -1 for the interval i. Finally, 
you need output the stored value of each interval as an array.
```java
class Interval {
    int start;
    int end;
    Interval(int s, int e) { start = s; end = e; }
}
```

Note:
* You may assume each interval's end point is **always bigger** than its start point. 就是说任何interval都不会长度为0
* You may assume **none of these intervals have the same start point**. 这一点对于代码的实现方式和有影响，具体见下面的code

### Example
* Input: [ [1,2] ]
  * Output: [-1]
    * There is only one interval in the collection, so it outputs -1.
* Input: [ [3,4], [2,3], [1,2] ]
  * Output: [-1, 0, 1]
    * There is no satisfied "right" interval for [3,4]。 For [2,3], the interval [3,4] has minimum-"right" start point. For [1,2], the interval [2,3] has minimum-"right" start point.
* Input: [ [1,4], [2,3], [5,7] ]
  * Output: [-1, 2, -1]
    * There is no satisfied "right" interval for [1,4] and [3,4]. For [2,3], the interval [5,7] has minimum-"right" start point.

## Solution 1: 我的朴素方法。速度慢。回头看下有没有更快的方法 ？！？
* 要把所有interval根据他们的start point 进行从小到大的排序，这样才有利于之后的处理。但我们不能丢掉每个interval在初始数组里的位置即index，所以在排序
之前，我们要先做下面这一步：
* 用一个map记录每个interval和它们的初始indexes的对应关系
* 对intervals进行排序
* 对于排序后的每个interval，取出它的end point，然后对interval数组进行二分查找，找的interval需要满足：它的start point要大于或等于给定的end point，
然后还必须是最小的那一个start point

### Complexity
* Time: O(n logn)
* Space: O(n)

### Java
代码看起来不短，其实逻辑较简明
```java
public class Solution {
    public int[] findRightInterval(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return new int[0];
        }
        
        int n = intervals.length;
        int[] result = new int[n];
        
        // 我们需要一个map来存每个interval在最初的array里的index。
        // 因为题目说了，没有任何interval有相同的start point，所以map的key可以是start point，
        // 即int，而不必是interval，后者要做key的话就需要些写equals()和hashcode()这两个函数
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(intervals[i].start, i);
        }
        
        // 所有的interval按照start point从小到大排列
        Arrays.sort(intervals, new Comparator<Interval>() {
            @Override
            public int compare(Interval itv1, Interval itv2) {
                if (itv1.start == itv2.start) {
                    return 0;
                }
                return itv1.start > itv2.start ? 1 : -1;
            }
        });
        for (Interval itv : intervals) {
            System.out.println(itv.start + ", " + itv.end);
        }
        
        for (int i = 0; i < n; i++) {
            int curStart = intervals[i].start;
            int curEnd = intervals[i].end;
            int originalIndex = map.get(curStart);

            Interval smallestStartInterval = 
                    findSmallestStartAfterTargetEnd(intervals, i, curEnd);

            if (smallestStartInterval == null) {
                result[originalIndex] = -1;
                continue;
            }
            result[originalIndex] = map.get(smallestStartInterval.start);
        }
        return result;
    }

    // 用二分法查找
    // curIndex的意思是，目前给的这个end所属的interval的index，
    // 那么我们要找的interval一定是在当前这个interval的后面的，所以left从curIndex开始向右走
    private Interval findSmallestStartAfterTargetEnd(
            Interval[] intervals, int curIndex, int end) {
        int n = intervals.length;
        
        // 先处理一下特殊情况，即最后一个interval的start也比我们给的end要小，
        // 这也处理了本身就是最后一个interval的情况
        if (intervals[n - 1].start < end) {
            return null;
        }
        
        int left = curIndex, right = intervals.length - 1;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if (intervals[mid].start >= end) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        // 最后left和right应该是重合的
        return intervals[left];
    }
}
```

## Solution 2: 用TreeMap做，速度较快。学习一下别人对TreeMap的运用方式！
Ref: https://discuss.leetcode.com/topic/65817/java-clear-o-n-logn-solution-based-on-treemap

TreeMap: A Red-Black tree based NavigableMap implementation. 
The map is sorted according to the natural ordering of its keys, or by a Comparator provided at map creation time, 
depending on which constructor is used.

### Complexity
* Time: O(n logn)
* Space: O(n)

### Java
```java
public class Solution {
    public int[] findRightInterval(Interval[] intervals) {
        int len = intervals.length;
        int[] result = new int[len];
        TreeMap<Integer,Integer> intervalTreeMap = new TreeMap<>();
        
        for (int i = 0; i < len; i++)
            intervalTreeMap.put(intervals[i].start, i);
            
        for (int i = 0; i < len; i++)
        {
            // ceilingEntry(K key)
            // Returns a key-value mapping associated with the least key greater than or equal to 
            // the given key, or null if there is no such key
            Map.Entry<Integer,Integer> entry = intervalTreeMap.ceilingEntry(intervals[i].end);
            if (entry != null)
                result[i] = entry.getValue();
            else // null
                result[i] = -1;
        }
        return result;
    }
}
```

## Reference
* [Find Right Interval [LeetCode]](https://leetcode.com/problems/find-right-interval/description/)

{% include links.html %}
