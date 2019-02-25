---
title: "Employee Free Time"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_employee_free_time.html
folder: algorithm
toc: false
---

## Description
We are given a list schedule of employees, which represents the working time for each employee.
Each employee has a list of non-overlapping Intervals, and these intervals are in sorted order.
```java
 class Interval {
     int start;
     int end;
     Interval() { start = 0; end = 0; }
     Interval(int s, int e) { start = s; end = e; }
 }
```
Return the list of finite intervals representing **common, positive-length free time** for all employees, also in sorted order. 这题虽然被LC标为hard题，其实不难，应该说是median题

Note:
* schedule and schedule[i] are lists with lengths in range [1, 50]. 0 <= schedule[i].start < schedule[i].end <= 10^8. All intervals have positive length
* The length of each free time span must be positive, zero length is not accepted as a free time span

### Example
* Input: schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]
  * Output: [[3,4]]. There are a total of three employees, and all common free time intervals would be [-inf, 1], [3, 4], [10, inf]. We discard any intervals that contain inf as they aren't finite.

## Solution 1：用 PriorityQueue，速度前 10%（如果用lambda表达式反而更慢）
这题既然要求所有人共有的 free time，则一段 working interval到底是谁的就没意义了，所有人的所有interval都可以并且都应该搞在一起

Ref: https://leetcode.com/problems/employee-free-time/discuss/113134/Simple-Java-Sort-Solution-Using-(Priority-Queue)-or-Just-ArrayList

The idea is to just add all the intervals to the priority queue. (NOTE that it is not matter how many different people are there for the algorithm. becuase we just need to find a gap in the time line.
* Priority queue - sorted by start time, 不必对end time 排序！实测ok！
* Everytime you poll from priority queue, just make sure it doesn't intersect with previous interval.
This mean that there is no coomon interval. Everyone is free time.

### Complexity
* Time: O(nlogn)
* Space: O(n)

### Java
下面的代码里，写了 lambda 表达式，这是两层的判断嵌套，如果只有一层，那么这个lambda表达式还可以简略很多。

lambda表达式比老老实实写 PQ 的 Comparator 会省很多代码。不过LC上实测运算速度，PQ Comparator 那么写反而会比lambda 快很多
```java
class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
    	List<Interval> result = new ArrayList<>();
	if (schedule == null || schedule.size() == 0) {
	    return result;
        }
	
	PriorityQueue<Interval> pq = new PriorityQueue(10, new Comparator<Interval>(){
	    @Override
            public int compare(Interval itv1, Interval itv2) {
                if (itv1.start == itv2.start) {
		    return 0;
                }
                return itv1.start > itv2.start ? 1 : -1;
            }
        });
        
        /* lambda 表达式，先升序排序start，再升序排序end的写法。其实不用排序end
        PriorityQueue<Interval> pq = new PriorityQueue<>(10, (Interval i1, Interval i2) -> 
            (i1.start - i2.start == 0 ? i1.end - i2.end : i1.start - i2.start)
        ); */

        // put all intervals into the priority queue
        for (List<Interval> list : schedule) {
            for (Interval interval : list) {
                pq.add(interval);
            }
        }

	Interval merged = pq.poll(); // 先把第一个搞出来
	while (!pq.isEmpty()) {
	    int curStart = pq.peek().start;
	    int curEnd = pq.peek().end;

            // 出现了 common free time！
            if (merged.end < curStart) {
                Interval freeTime = new Interval(merged.end, curStart);
                result.add(freeTime);
                
                merged = pq.poll(); // 别忘了这个
                continue;
            }

            // merged.end >= curStart
            if (merged.end < curEnd) {
                merged.end = curEnd;
            }
            pq.poll(); // 当前pq顶部的interval已经完成了使命
        }
        return result;
    }
}
```

如果不用PQ，用List也可以。代码看起来会更简明，但LC实测速度不如 PQ 快。虽然理论上它们都是 nlogn 的速度
```java
public class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> result = new ArrayList<>();
        if (schedule == null || schedule.size() == 0) {
            return result;
        }

        List<Interval> intervals = new ArrayList<>();
        for (List<Interval> list : schedule) {
            for (Interval itv : list) {
                intervals.add(itv);
            }
        }
        
        // 用lambda表达式进行排序，逼格满满
        Collections.sort(intervals, 
                         (Interval itv1, Interval itv2) -> (itv1.start - itv2.start));
        
        Interval prev = intervals.get(0);
        
        for (int i = 1; i < intervals.size(); i++) {
            Interval cur = intervals.get(i);
            
            if (prev.end >= cur.start) {
                prev.end = Math.max(prev.end, cur.end);
                continue;
            }
            
            Interval freeTime = new Interval(prev.end, cur.start);
            result.add(freeTime);
            prev = cur;
        }
        return result;
    }
}
```

## Solution 2: Sweep Line (扫描线) 很精妙的方法，再加上 TreeMap。但LC实测速度反而比朴素的PQ还要慢点
**遇到 start 就 +1，遇到 end 就 -1，为 0 的时候就是所有人都 free 的时候！**

### Complexity
* Time: O(nlogn)，把所有东西都放到TreeMap里是 nlogn 的时间，扫一遍是 n 的时间
* Space: O(n)

### Java
```java
public class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> result = new ArrayList<>();
        if (schedule == null || schedule.size() == 0) {
            return result;
        }

        // TreeMap就是把BST和Map结合在一起，
        // 所以一方面它可以秒search key，另一方面 它的 keys 有一定的顺序！(靠BST)
        // 这个TreeMap里的key是位置点，可能是start也可能是end，一视同仁！value是出现的次数，
        // 看下面代码就可知为何这么设置
        TreeMap<Integer, Integer> tm = new TreeMap<>();

        // 把所有的intervals都放到TreeMap里去
        for (List<Interval> list : schedule) {
            if (list == null) continue;

            for (Interval itv : list) {
                // 这一题的关键在此！遇到start则+1，遇到end则-1！
                tm.put(itv.start, tm.getOrDefault(itv.start, 0) + 1);
                tm.put(itv.end, tm.getOrDefault(itv.end, 0) - 1);
            }
        }

        int freeCount = 0;
        int freeStart = -1, freeEnd = -1;

        for (Integer key: tm.keySet()) {
            freeCount += tm.get(key);

            if (freeCount == 0) {
                freeStart = key;
            } 
            // freeCount > 0。注意 freeCount是不会 < 0 的
            // freeStart != -1 意味着当前正有一段free time 在进行中
            else if (freeStart != -1) { 
                freeEnd = key;

                result.add(new Interval(freeStart, freeEnd));
                freeStart = -1;
            }
        }
        return result;
    }
}
```

## Reference
* [Employee Free Time [LeetCode]](https://leetcode.com/problems/employee-free-time/description/)

{% include links.html %}
