---
title: "Maximum Vacation Days among Cities with Flights"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_max_vacation_days_among_cities.html
folder: algorithm
toc: false
---

## Description
LeetCode wants to give one of its best employees the option to travel among N cities to collect algorithm problems. 
But all work and no play makes Jack a dull boy, you could take vacations in some particular cities and weeks. 
Your job is to schedule the traveling to maximize the number of vacation days you could take, but there are certain rules and restrictions you need to follow.

**So you start at city 0, but for the 1st week you can stay at city 0 or any city that can be reached to from city 0. 
So for the 1st week, there might be some cities that are not reachable, 
and for all the weeks, there might be some cities that are not reachable. And it is also possible that there is no any flights 
between any cities, so we will have to stay at city 0 always.**

**The number of vacation days of any city in any week can be ZERO.**

* Rules and restrictions
  * You can only travel among N cities, represented by indexes from 0 to N-1. Initially, you are in the city indexed 0 on Monday.
  * The cities are connected by flights. The flights are represented as a N*N matrix (not necessary symmetrical), called flights representing the airline status from the city i to the city j. If there is no flight from the city i to the city j, flights[i][j] = 0; Otherwise, flights[i][j] = 1. Also, flights[i][i] = 0 for all i.
  * You totally have K weeks (each week has 7 days) to travel. You can only take flights at most once per day and can only take flights on each week's Monday morning. Since flight time is so short, we don't consider the impact of flight time.
  * For each city, you can only have restricted vacation days in different weeks, given an N*K matrix called days representing this relationship. For the value of days[i][j], it represents the maximum days you could take vacation in the city i in the week j.

You're given the flights matrix and days matrix, and you need to output the maximum vacation days you could take during K weeks.

### Example
* Input: 
  * Output: 

## Solution: DP
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [Maximum Vacation Days [LeetCode]](https://leetcode.com/problems/maximum-vacation-days/)

{% include links.html %}
