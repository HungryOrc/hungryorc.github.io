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
* 第一次，从左往右走一遍，考察相邻的两个人，如果靠前的一个认识靠后的一个，则靠前的一个一定不是cele，靠后那个**可能**是cele。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution extends Relation {
    boolean found = false;
    
    public int findCelebrity(int n) {
        if (n <= 0) {
            return -1;
        }
        
        int candidate = 0;
        for (int i = 1; i < n; i++) {
            if (knows(candidate, i)) {
                candidate = i;
            }
        } // else keep the current candidate
        
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
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
