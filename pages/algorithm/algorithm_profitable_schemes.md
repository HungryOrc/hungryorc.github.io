---
title: "Profitable Schemes"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_profitable_schemes.html
folder: algorithm
toc: false
---

## Description
There are G people in a gang, and a list of various crimes they could commit.

The i-th crime generates a profit[i] and requires group[i] gang members to participate.

If a gang member participates in one crime, that member can't participate in another crime.

Let's call a profitable scheme any subset of these crimes that generates at least P profit, and the total number of gang members participating in that subset of crimes is at most G.

How many schemes can be chosen?  Since the answer may be very large, return it modulo 10^9 + 7. 这是本题的一个特殊要求。和本题的主题逻辑没有任何关系
* 1 <= G <= 100
* 0 <= P <= 100
* 1 <= group[i] <= 100
* 0 <= profit[i] <= 100
* 1 <= group.length = profit.length <= 100

### Example
* Input: G = 5, P = 3, group = [2,2], profit = [2,3]
  * Output: 2
  * To make a profit of at least 3, the gang could either commit crimes 0 and 1, or just crime 1. In total, there are 2 schemes.
* Input: G = 10, P = 5, group = [2,3,5], profit = [6,7,8]
  * Output: 7
  * To make a profit of at least 5, the gang could commit any crimes, as long as they commit one. There are 7 possible schemes: (0), (1), (2), (0,1), (0,2), (1,2), and (0,1,2).

## Solution: DP
这题比较明显是要用DP做，难度是 hard。具体处理有一些需要注意的地方。题意中有2个限制，一个是总人数有上限，一个是总利润有下限，自然想起背包问题，
总重量有上限，总价值有下限那样的题目。
* `int[][] dp = new int[G + 1][allProfitSum + 1]`, 其中 `dp[i][j]` 意思是 正好用了i个人，利润正好是j，一共有多少种方法
* 递推关系：dp[memberNumberSum][profitSum] += dp[memberNumberSum - curMemberNumber][profitSum - curProfit]
* 初始状态：dp[0][0] = 1，即没人也没利润，有一种方式，自然就是不派人也不做事。这个看起来很无厘头，也不好想。但是非常重要！这相当于一生万物的那个一，
没有它，后面的一切都是零，都无从发动
* 特别注意，初始状态不要包括 `dp[每个groupMemberNumber][相应的每个profit] = 1`！
  * 这个也不好想，反直觉。习惯来说应该是初始把这一串都列上的。但按照我们的递推，就不能把数组 `group` 和 `profit` 里的一对一对的数列在初始条件里。如果那样做，上面的递推式子就会重复！
  * 比如 4人2利润 是一个任务，3人5利润 是另一个任务。那么 7人7利润 应该是有一种方法。但是如果把 4人2利润 和 3人5利润 都列在初始条件里都设为1（种方法），那么 7人7利润 最后就会被加成2种方法。但其实只有一种，即前面两个任务都做才能达成7,7


### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [Profitable Schemes [LeetCode]](https://leetcode.com/problems/profitable-schemes/description/)

{% include links.html %}
