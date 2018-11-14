---
title: "Palindrome Linked List"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_palindrome_linked_list.html
folder: algorithm
toc: false
---

## Description
Given a singly linked list, determine if it is a palindrome.
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

### Follow up
Could you do it in O(n) time and O(1) space?

### Example
* Input: 1->2->2->1 
  * Output: true

## Solution
三步走，见下面代码

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        
        // Step 1, get the head of the right half list
        ListNode rightHead = getRightHead(head);
        
        // Step 2, reverse the right half list
        ListNode newRightHead = reverseList(rightHead);
        
        // Step 3, compare each node in the two halves
        return compareTwoLists(head, newRightHead);
    }
    
    private ListNode getRightHead(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
    
    private ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode next = head.next;
        ListNode newHead = reverseList(next);
        
        next.next = head;
        head.next = null;
        
        return newHead;
    }
    
    // 按照这一题的情况，两个list的长度最多差 1
    private boolean compareTwoLists(ListNode h1, ListNode h2) {
        while (h1 != null && h2 != null) {
            if (h1.val != h2.val) {
                return false;
            }
            h1 = h1.next;
            h2 = h2.next;
        }
        return true;
    }
}
```

## Reference
* [Palindrome Linked List [LeetCode]](https://leetcode.com/problems/palindrome-linked-list/description/)

{% include links.html %}
