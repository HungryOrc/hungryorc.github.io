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
哦也

这题虽然被LC标为hard题，其实不难，应该说是median题

### Example
* Input: 
  * Output: 

## Solution 1：用 PriorityQueue
Ref: https://leetcode.com/problems/employee-free-time/discuss/113134/Simple-Java-Sort-Solution-Using-(Priority-Queue)-or-Just-ArrayList

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
* Time: O(nlogn)
* Space: O(n)

### Java
```java

```

## Reference
* [Employee Free Time [LeetCode]](https://leetcode.com/problems/employee-free-time/description/)

{% include links.html %}
