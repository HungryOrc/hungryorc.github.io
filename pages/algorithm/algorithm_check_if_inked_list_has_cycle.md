---
title: "Check if a Linked List Has at Least One Cycle in Itself"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_check_if_inked_list_has_cycle.html
folder: algorithm
toc: false
---

## Description
Given a linked list, determine if it has a cycle in it.

**Solve it without using extra space!**

```
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

### Example
略

## Solution
用快慢2个指针，slowPointer一次走一步，fastPointer一次走两步
* 如果不循环，则fast一定会撞到null，且一定不会撞到slow
* 如果循环，则fast一定不会撞到null，且迟早一定会撞到slow
  * 如果我们设fast一次走超过2步比如n步，也一定会撞到slow，只是撞到slow的时间可能会更长也可能会缩短，并不一定会缩短！

**关键点：快的从一开始就先多走一步！这样整个处理会简化不少！**

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null)
            return false;
        
        // 快的从一开始就先多走一步！不然后面的 while(fast != slow) 根本无法开始！
        ListNode fast = head.next;
        ListNode slow = head;
        
        while(fast != slow) {
            if (fast.next == null || fast.next.next == null) {
                return false;
            } else {
                fast = fast.next.next;
                slow = slow.next;
            }
        }
        return true; // this is when fast == slow
    }
}
```

## Reference
* [Linked List Cycle [LeetCode]](https://leetcode.com/problems/linked-list-cycle/description/)

{% include links.html %}
