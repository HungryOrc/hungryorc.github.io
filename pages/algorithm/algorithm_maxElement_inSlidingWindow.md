---
title: "Max Element in Sliding Window"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_maxElement_inSlidingWindow.html
folder: algorithm
toc: false
---

## Description
Given an integer array A and a sliding window of size K, 
find the maximum value of each window as it slides from left to right.

Assumptions:
* The given array is not null and is not empty
* K >= 1, K <= A.length

### Example
* Input: A = {1, 2, 3, 2, 4, 2, 1}, K = 3
  * Output: the windows are `{1,2,3}, {2,3,2}, {3,2,4}, {2,4,2}, {4,2,1}`, so the maximum values of each K-sized sliding window are [3, 3, 4, 4, 4]

## Solution: 单调栈(单调下降)。两头读写，用Deque实现
自定义一个 element class，每个element包括原数组里的 int element的value，以及原int在int array里的index。对照着原int数组搞一个element数组

然后，对于element数组，从左到右，每新来一个 原数组里的 int element，就：
* 先看当前deque尾部（右端）的元素的value，是否比cur的value要小，如果是，则用poll last消灭这些元素。
  * 这么做是因为：有cur在，这些元素既比cur小，又在cur的前面，所以这些元素必然永远也翻不了身，不可能成为任何sliding window的max。
* 然后把cur放在deque的尾部（最右端）
* 再看当前deque头部（左端）的元素的index，是否超过了sliding window长度的左界限。如果超过了，就用poll first来消灭掉，
  * 它们如果超越了window的界限，自然不可能留下
* 遮掩就构成了一个 **单调下降栈！**
* 然后把当前deque的头部的元素的value放到result list里去

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Element {
    int val;
    int index;
    public Element(int val, int index) {
        this.val = val;
        this.index = index;
    }
}

public class Solution {
    public List<Integer> maxWindows(int[] array, int k) {
        List<Integer> result = new ArrayList<>();
        int n = array.length;
        
	Element[] elements = new Element[n];
        for (int i = 0; i < n; i++) {
            Element ele = new Element(array[i], i);
            elements[i] = ele;
        }

        Deque<Element> deque = new LinkedList<>(); 
	// deque是空的，把element array 里的东西一个一个往里面加

        for (int i = 0; i < n; i++) {
            Element cur = elements[i];
            
	    while (!deque.isEmpty() && cur.val > deque.peekLast().val) {
                deque.pollLast();
            }
	    
	    // 无论怎样，都要把cur加到deque的右端
            deque.offerLast(cur);
	
            if (i >= k - 1) { // 足够长了才会开始扔走左边头部的元素
                while (!deque.isEmpty() && deque.peekFirst().index <= i - k) {
                    deque.pollFirst();
                }
		
		// 这里只是peek而已！不是pop
                result.add(deque.peekFirst().val);
            }
        }
        return result;
    }
}
```

## Reference
没在网上找这题

{% include links.html %}
