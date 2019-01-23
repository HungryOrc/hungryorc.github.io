---
title: "Find the celebrity who does exist"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_find_celebrity_who_does_exist.html
folder: algorithm
toc: false
---

## Description
有n个人，编号0到n-1。里面一定有一个celebrity，作为cele，就是说ta不认识这n个人里的任何其他人，但所有其他人都认识ta。B格满格。
要求返回cele的编号。要求：时间 O(n)！

The `knows` API is defined in the parent class `Relation`:
```
boolean knows(int a, int b);
```

相似题目：Find the Celebrity if any ---- 在LeetCode上有，我也总结在这个笔记里了。

### Example
略

## Solution
这一题的难点在于第一如何快速的排除不是cele的人，以做到用O(n)的时间得到答案。

注意，如果存在cele，则最多存在一个cele。可以用反证法证明：如果存在2个cele，双方必须都不认识对方，双方也必须都认识对方，则矛盾。

从左往右 walk through 所有人，每次考察相邻的两个人：
* 如果靠前的人认识靠后的人，则靠前的人一定不是cele，靠后的人**可能**是cele，那么就把cele的candidate设为靠后那个人
* 如果靠前的人不认识靠后的人，则靠后的人一定不是cele，靠前的人**可能**是cele，那么就把cele的candidate设为靠前那个人
* 最后这么得出的人，一定就是那个cele。因为上面的过程决定了，其他人都不可能是cele

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
        
        int candidate = 0;
        for (int i = 1; i < n; i++) {
            if (knows(candidate, i)) {
                candidate = i;
            } // else keep the current candidate
        }

        return candidate;
    }
}
```

## Reference
这题还没找到网上有可练习的题目

{% include links.html %}
