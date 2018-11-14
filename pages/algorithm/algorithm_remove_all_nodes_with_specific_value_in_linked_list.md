---
title: "Remove All the Nodes with a Specific Value in the Linked List"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_remove_all_nodes_with_specific_value_in_linked_list.html
folder: algorithm
toc: false
---

## Description
Remove all elements from a linked list of integers that have value `val`.
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```
注意，需要删除的node有可能在list的头部，或者尾部，或者中间。

### Example
* Input: 1->2->6->3->4->5->6, val = 6
  * Output: 1->2->3->4->5

## Solution 1, Iteration

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyHead = new ListNode(Integer.MIN_VALUE);
        dummyHead.next = head;
        
        ListNode prev = dummyHead;
        ListNode cur = head;
        
        while (cur != null) {
            if (cur.val == val) {
                prev.next = cur.next;
            } else {
                prev = cur;
            }
            cur = cur.next;
        }
        
        return dummyHead.next;
    }
}
```

## Solution 1, Recursion，很巧妙，我是没想到
Ref: https://discuss.leetcode.com/topic/12725/ac-java-solution

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return null;
        }
        
        // 很妙
        head.next = removeElements(head.next, val); 
        // 这里用了head.next，所以之前不得不判断下head是否为null
        
        // 也很妙
        if (head.val == val) {
            return head.next;
        }
        return head;
    }
}
```

## Reference
* [Remove Linked List Elements [LeetCode]](https://leetcode.com/problems/remove-linked-list-elements/description/)

{% include links.html %}
