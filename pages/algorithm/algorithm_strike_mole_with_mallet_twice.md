---
title: "Whac a Mole: Strike Moles with a Mallet, Twice"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_strike_mole_with_mallet_twice.html
folder: algorithm
toc: false
---

## Description
给一个int数组，比如 [0,1,0,1,1,1,0,1]，其中1代表一个鼹鼠mole，0代表没有鼹鼠。设锤子mallet的宽度是m。只让砸两锤，问最多能砸中几个鼹鼠。
* 默认int数组长度大于mallet的宽度

如果只让砸一锤，那么就是一个sliding window max sum的问题，很简单。砸两锤，要求实现最优的O(n)解法的话，就需要用DP。

### Example
* Input: [0,1,0,1,1,1,0,1], m = 3
  * Output: 4

## Solution
* 第一遍从左到右遍历整个数组：得到在不同的startIndex上锤一下，能锤中几个mole。这样得到一个新数组 int[] counts
* 第二遍，从左到右遍历 counts 数组，得到它里面每个index左边最大的元素有多大
* 第三遍，从右到左遍历 counts 数组，得到它里面每个index右边最大的元素有多大
* 最后一遍，左边最大 + 右边最大，用左右分段的DP方法解决问题
* 上面每一遍都是 O(n) 的时间。所以最后也是 O(n)

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    public int strikeMolesTwice(int[] moles, int malletWidth) {
        if (moles == null || moles.length == 0) {
            return 0;
        }
        
        // 默认int数组长度大于mallet的宽度
        int len = moles.length;
        int[] counts = new int[len];
        int[] maxBeforeAndAtIndex = new int[len];
        int[] maxAfterIndex = new int[len];
        
        // write the int array "counts"
        for (int i = 0; i < malletWidth; i++) {
            counts[0] += moles[i];
        }
        for (int i = malletWidth; i < len; i++) {
            counts[i] += moles[i];
            counts[i] -= moles[i - malletWidth];
        }
        
        maxBeforeAndAfterindex = getLeftMax(counts);
        maxAfterIndex = getRightMax(counts);
        
        int maxHits = 0;
        for (int i = 0; i < n; i++) {
            maxHit = Math.max(maxHit, maxBeforeAndAfterindex[i] + maxAfterIndex[i]);
        }
        return maxHits;
    }

}
```

## Reference
* 网上没有搜到这题

{% include links.html %}
