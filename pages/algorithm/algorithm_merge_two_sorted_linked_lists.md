---
title: "Merge Two Sorted Linked Lists"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_merge_two_sorted_linked_lists.html
folder: algorithm
toc: false
---

## Description
Merge two sorted linked lists and return it as a new list. 
The new list should be made by splicing together the nodes of the first two lists.
```java
 public class ListNode {
     int val;
     ListNode next;
     ListNode(int x) { val = x; }
 }
```

### Example
* Input: 1->2->4, 1->3->4
  * Output: 1->1->2->3->4->4

## Solution 1: Recursion

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public ListNode mergeTwoLists(ListNode h1, ListNode h2) {
        if (h1 == null) {
            return h2;
        }
        if (h2 == null) {
            return h1;
        }
        
        if (h1.val <= h2.val) {
            h1.next = mergeTwoLists(h1.next, h2);
            return h1;
        } else {
            h2.next = mergeTwoLists(h1, h2.next);
            return h2;
        }
    }
}
```

## Reference
* [Merge Two Sorted Lists [LeetCode]](https://leetcode.com/problems/merge-two-sorted-lists/description/)

{% include links.html %}
