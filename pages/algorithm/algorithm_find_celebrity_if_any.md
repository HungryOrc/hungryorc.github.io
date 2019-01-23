---
title: "Find the celebrity if any"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_celebrity_if_any.html
folder: algorithm
toc: false
---

## Description
Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) which tells you whether A knows B. Implement a function int findCelebrity(n), your function should minimize the number of calls to knows.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. **If there is no celebrity, return -1.**

The `knows` API is defined in the parent class `Relation`:
```
boolean knows(int a, int b);
```

### Example
略

## Solution
这一题的难点在于第一如何快速的排除不是cele的人。第二如果不存在cele，如何核实。

注意，如果存在cele，则最多存在一个cele。可以用反证法证明：如果存在2个cele，双方必须都不认识对方，双方也必须都认识对方，则矛盾。

这题可以用walk through所有人两次的方法来解决。第一次，找出疑似cele的人。第二次，核实此人到底是不是cele。具体来说：
* 第一次遍历，从左往右走一遍，考察相邻的两个人，
  * 如果靠前的人认识靠后的人，则靠前的人一定不是cele，靠后的人**可能**是cele，那么就把cele的candidate设为靠后那个人
  * 如果靠前的人不认识靠后的人，则靠后的人一定不是cele，靠前的人**可能**是cele，那么就把cele的candidate设为靠前那个人
  * 最后这么得出的人，未必是cele。因为上面的流程没有反向核实，即如果前后两个人都认识对方，或者都不认识对方，则选出来的candidate就不是cele。
    但是如果真有一个cele，则一定是最后得到的这个人，其他人都不可能是cele
  * 我曾经尝试过在一次遍历里就把筛选和复核都做了，但试了几种方式都没成功
* 第二次遍历，就简单了，就是把所有人和cele candidate交合一下，看是否满足ta认识cele但cele不认识ta。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution extends Relation {
    
    public int findCelebrity(int n) {
        if (n <= 0) {
            return -1;
        }
        
        // 第一步，筛查可能的cele
        int candidate = 0;
        for (int i = 1; i < n; i++) {
            if (knows(candidate, i)) {
                candidate = i;
            } // else keep the current candidate
        }
        
        // 第二步，复查筛出来的这个cele到底是不是cele
        for (int i = 0; i < n; i++) {
            if (i == candidate) {
                continue;
            }
            if (knows(candidate, i) || !knows(i, candidate)) {
                return -1;
            }
        }
        
        return candidate;
    }
}
```

## Reference
* [Find the Celebrity [LeetCode]](https://leetcode.com/problems/find-the-celebrity/description/)

{% include links.html %}
