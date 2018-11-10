---
title: "Find the Middle Node of a Linked List"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_middle_node_of_linked_list.html
folder: algorithm
toc: false
---

## Description
Given a non-empty, singly linked list with head node `head`, return a middle node of linked list.

If there are two middle nodes, return the **second** middle node.

```
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

### Example
* Input: [1, 2, 3, 4, 5]
  * Output: 3
* Input: [1, 2, 3, 4]
  * Output: 3

## Solution
快慢指针

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow; // 奇数偶数情况相同
    }
}
```

## Reference
* [Middle of the Linked List [LeetCode]](https://leetcode.com/problems/middle-of-the-linked-list/description/)

{% include links.html %}
