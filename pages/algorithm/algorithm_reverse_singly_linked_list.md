---
title: "Reverse Singly Linked List"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_reverse_singly_linked_list.html
folder: algorithm
toc: false
---

## Description
Reverse a singly linked list. 
* Do it both iteratively and recursively
* Definition for singly-linked list.
  ```
  public class ListNode {
      int val;
      ListNode next;
      ListNode(int x) { val = x; }
  }
  ```

### Example
* Input: `1->2->3->4->5->NULL`
  * Output: `5->4->3->2->1->NULL`

## Solution 1: Recursion, with a helper function

### Complexity
* Time: O(n)
* Space: O(n)，因为有n层call stack

### Java
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        
        return reverse(null, head, head.next);
    }
    
    private ListNode reverse(ListNode prev, ListNode cur, ListNode next) {
        cur.next = prev;
        
        if (next == null) {
            return cur;
        }
        
        return reverse(cur, next, next.next);
    }
}
```

## Reference
* [Reverse Linked List [LeetCode]](https://leetcode.com/problems/reverse-linked-list/description/)

{% include links.html %}
