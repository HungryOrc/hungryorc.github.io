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
This is a **Hard** level question.

* Note
  * N and K are positive integers, which are in the range of [1, 100].
  * In the matrix flights, all the values are integers in the range of [0, 1].
  * In the matrix days, all the values are integers in the range [0, 7].
  * You could stay at a city beyond the number of vacation days, but you should work on the extra days, which won't be counted as vacation days.
  * If you fly from the city A to the city B and take the vacation on that day, the deduction towards vacation days will count towards the vacation days of city B in that week.
  * We don't consider the impact of flight hours towards the calculation of vacation days.

### Example
* Input: flights = [[0,1,1],[1,0,1],[1,1,0]], days = [[1,3,1],[6,0,3],[3,3,3]]
  * Output: 12. 6 + 3 + 3 = 12
  * One of the best strategies is:
    * 1st week : fly from city 0 to city 1 on Monday, and play 6 days and work 1 day. (Although you start at city 0, we could also fly to and start at other cities since it is Monday.) 
    * 2nd week : fly from city 1 to city 2 on Monday, and play 3 days and work 4 days.
    * 3rd week : stay at city 2, and play 3 days and work 4 days.
* Input: flights = [[0,1,1],[1,0,1],[1,1,0]], days = [[7,0,0],[0,7,0],[0,0,7]]
  * Output: 21. 7 + 7 + 7 = 21
  * One of the best strategies is:
    * 1st week : stay at city 0, and play 7 days. 
    * 2nd week : fly from city 0 to city 1 on Monday, and play 7 days.
    * 3rd week : fly from city 1 to city 2 on Monday, and play 7 days.

## Solution: DP
* `int dp[i][j]` means: we are **staying at city i in week j**, the max vacation days we can get from week 0 to week j, with any possible flights/stays since week 0
* Attention: when filling up the dp matrix, **the outer for loop must be the weeks!** The inner loop are the cities.

### Complexity
* Time: O(numOfWeeks * numOfCities * numOfCities)
* Space: O(numOfWeeks * numOfCities)

### Java
```java
class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {
        // ignore the validation of parameters
        
        int numCities = flights.length;
        int numWeeks = days[0].length;
        
        int[][] dp = new int[numCities][numWeeks];
        
        // since the number of holidays in any week in any city can
        // be 0, we have to set all elements to be -1 in advance
        // to distinguish the "unreachable" (city, week) state to
        // the "zero holiday" (city, week) state
        for (int i = 0; i < numCities; i++) {
            for (int j = 0; j < numWeeks; j++) {
                dp[i][j] = -1;
            }
        }
        
        // base case: for the 1st week:
        // we start at city 1
        dp[0][0] = days[0][0];
        // we can also stay at other cities in the 1st week,
        // if city 1 can fly to that city
        for (int i = 1; i < numCities; i++) {
            if (flights[0][i] == 1) {
                dp[i][0] = days[i][0];
            }
        }
        
        // the outer loop must be the weeks! the inner loop is cities,
        // since this problem is developed by time,
        // so we have to fill in the dp matrix by column, from left to right!
        
        // starting from the 2nd week
        for (int j = 1; j < numWeeks; j++) {
                
            // for each city
            for (int i = 0; i < numCities; i++) {
            

                // if it is possible to staty at city i in week j-1
                if (dp[i][j - 1] != -1) {
                    dp[i][j] = dp[i][j - 1] + days[i][j];
                }
                
                for (int prevCity = 0; prevCity < numCities; prevCity++) {
                    // if it's possible to stay a this prevCity in week j-1,
                    // and this prevCity has a flight to fly to city i
                    if (dp[prevCity][j - 1] != -1 && flights[prevCity][i] == 1) {
                        dp[i][j] = Math.max(dp[i][j],
                                            dp[prevCity][j - 1] + days[i][j]);
                    }            
                }
            }
        }
        
        int maxVDays = 0;
        for (int i = 0; i < numCities; i++) {
            maxVDays = Math.max(maxVDays, dp[i][numWeeks - 1]);
        }
        return maxVDays;
    }
}
```

## Reference
* [Maximum Vacation Days [LeetCode]](https://leetcode.com/problems/maximum-vacation-days/)

{% include links.html %}
