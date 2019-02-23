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

## Solution 1：用 PriorityQueue
Ref: https://leetcode.com/problems/employee-free-time/discuss/113134/Simple-Java-Sort-Solution-Using-(Priority-Queue)-or-Just-ArrayList

The idea is to just add all the intervals to the priority queue. (NOTE that it is not matter how many different people are there for the algorithm. becuase we just need to find a gap in the time line.
* Priority queue - sorted by start time, and for same start time sort by either largest end time or smallest (it is not matter).
* Everytime you poll from priority queue, just make sure it doesn't intersect with previous interval.
This mean that there is no coomon interval. Everyone is free time.

### Complexity
* Time: O(nlogn)
* Space: O(n)

### Java
```java
public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
	List<Interval> result = new ArrayList<>();

	PriorityQueue<Interval> pq = new PriorityQueue(10, new Comparator<Interval>(){
		@Override
		public int compare(Interval itv1, Interval itv2) {
			if (itv1.start == itv2.start) {
				if (itv1.end == itv2.end) {
	return 0;
}

lambda???? 表达式？？？
PriorityQueue<Interval> pq = new PriorityQueue<>(10, (Interval i1, Interval i2) -> (i1.start - i2.start == 0 ? i1.end - i2.end : i1.start - i2.start));


return itv1.end > itv2.end ? 1 : -1;
}
return itv1.start > itv2.start ? 1 : -1;
}
});

	for (List<Interval> list : schedule) {
		for (Interval interval : list) {
			pq.add(interval);
}
}

	Interval merged = pq.poll();
	while (!pq.isEmpty()) {
		int curStart = pq.peek().start;
		int curEnd = pq.peek().end;

		if (merged.end < curStart) {
			Interval gap = new Interval(merged.end, curStart);
			result.add(gap);
			merged = pq.poll();
			continue;
}

	if (merged.end < curEnd) {
		merged.end = curEnd;
}
pq.poll();
}
return result;
}
```

## Solution 2: Sweep Line，扫描线！很精妙的方法
**遇到 start 就 +1，遇到 end 就 -1，为 0 的时候就是所有人都 free 的时候！**

### Complexity
* Time: O(nlogn)，把所有东西都放到TreeMap里是 nlogn 的时间，扫一遍是 n 的时间
* Space: O(n)

### Java
```java
Hongchen <== 看不懂就问她！
public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
	List<Interval> ans = new ArrayList<>();
	if (schedule == null || schedule.size() == 0) {
		return ans;
}
	TreeMap<Integer, Integer> treemap = new TreeMap<>();
	for (List<Interval> list : schedule) {
		if (list == null) {
			continue;
}
		for (Interval i : list) {
			treemap.put(i.start, treemap.getOrDefault(i.start, 0) + 1);
			treemap.put(i.end, treemap.getOrDefault(i.end, 0) - 1);
}
}
int count = 0;
int start = 0, end = 0;//assuming 0 is not used in intervals
for (Integer key: treemap.keySet()) {
	count += treemap.get(key);
	if (count == 0) {
		start = key;
} else if (start != 0) {
	end = key;
	ans.add(new Interval(start, end));
	start = 0;
}
}
return ans;
}
```

```java
zhuqing <=== 不懂就问她！
public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
	List<Interval> res = new ArrayList<>();
	if (schedule == null || schedule.size() == 0) {
	return res;
}
      List<Interval> list = new ArrayList<>();
for (List<Interval> curr : schedule) {
	for (Interval interval : curr) {
	list.add(interval);
}
}
// Collections
Arrays.sort(list, (Interval l1, Interval l2) -> (l1.start - l2.start));
int prevEndTime = list.get(0).end;

for (int i = 1; i < list.size(); i++) {
	Interval curr = list.get(i);
	if (prevEndTime < curr.start) {
	res.add(new Interval(prevEndTime, curr.start));
prevEndTime = curr.end;
} else {
	prevEndTime = Math.max(curr.end, prevEndTime);
}
}
return res;
}
```

## Reference
* [Employee Free Time [LeetCode]](https://leetcode.com/problems/employee-free-time/description/)

{% include links.html %}
