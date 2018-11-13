---
title: "Merge Sort a Linked List"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_merge_sort_linked_list.html
folder: algorithm
toc: false
---

## Description
Given a singly-linked list, where each node contains an integer value, sort it in ascending order. 
The merge sort algorithm should be used to solve this problem.
```java
class ListNode {
    public int value;
    public ListNode next;
    public ListNode(int value) {
        this.value = value;
    }
}
```

### Example
* Input: 4 -> 2 -> 6 -> -3 -> 5
  * Output: -3 -> 2 -> 4 -> 5 -> 6

## Solution
三步走，如下面代码中所写

### Complexity
* Time: O(nlogn)，因为是 merge sort
* Space: O(logn)，因为有logn层的call stack

### Java
```java
public class Solution {
    public ListNode mergeSort(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
      
        // Step 1, get leftEnd and rightHead
        ListNode leftEnd = getLeftEnd(head);
        ListNode rightHead = leftEnd.next;
        leftEnd.next = null;
        
        // Step 2, sort the two halves respectively
        ListNode leftHead = mergeSort(head);
        rightHead = mergeSort(rightHead);
      
        // Step 3, merge two sorted lists
        // 最开始的sort也是在这里发生的，即对于2个长度为1的list的 sort & merge
        return mergeTwoSortedLists(leftHead, rightHead);
    }
  
    private ListNode getLeftEnd(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
      
        ListNode slow = head, fast = head.next;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
  
    private ListNode mergeTwoSortedLists(ListNode h1, ListNode h2) {
        if (h1 == null) {
            return h2;
        }
        if (h2 == null) {
            return h1;
        }
      
        if (h1.value <= h2.value) {
            h1.next = mergeTwoSortedLists(h1.next, h2);
            return h1;
        } else {
            h2.next = mergeTwoSortedLists(h1, h2.next);
            return h2;
        }
    }
}
```

## Reference
* [Merge Sort Linked List [LaiCode]](https://app.laicode.io/app/problem/29)

{% include links.html %}
