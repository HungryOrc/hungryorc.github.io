---
title: "Reorder List, One from Head and One from Tail"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_reorder_list_one_from_head_and_one_from_tail.html
folder: algorithm
toc: false
---

## Description
Given a singly linked list L: L0 → L1 → … → Ln-1 → Ln

Reorder it to: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

You may not modify the values in the list's nodes, only nodes itself may be changed.
```java
 public class ListNode {
     int val;
     ListNode next;
     ListNode(int x) { val = x; }
 }
```

### Example
* Input: 1->2->3->4
  * Output: 1->4->2->3
* Input: 1->2->3->4->5
  * Output: 1->5->2->4->3

## Solution
第一步，找mid node。第二步，reverse后半段。第三步，两个list merge到一起。这个方法的速度是 前 1%

关键：别忘了**把前半段的最后一个node的next设为null！** 不然会出现诡异错误！

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        
        // Step 1，找到left end 和right head
        ListNode leftEnd = getLeftEnd(head);
        ListNode rightHead = leftEnd.next;
        leftEnd.next = null; // 切记别忘了这个！
        
        // Step 2，reverse right half list
        ListNode newRightHead = reverseList(rightHead);
        
        // Step 3, merge 2 lists, 每次每边出一个node，不管大小
        mergeLists(head, newRightHead);
    }
    
    // 如果要 get right head 或者说 get middle node，
    // 那么就把slow和fast都初始化为 head；
    // 但如果是要 get left end，
    // 就把slow初始化为head，fast初始化为head.next
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
    
    private ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode nextNode = head.next;
        ListNode newHead = reverseList(nextNode);
        
        nextNode.next = head;
        head.next = null;
        
        return newHead;
    }
    
    private void mergeLists(ListNode head1, ListNode head2) {
        ListNode dummyHead = new ListNode(Integer.MIN_VALUE);
        
        while (head1 != null && head2 != null) {
            ListNode next1 = head1.next;
            ListNode next2 = head2.next;
            
            dummyHead.next = head1;
            head1.next = head2;
            
            dummyHead = head2; // 切记别忘了这个！
            head1 = next1;
            head2 = next2;
        }
        
        if (head1 == null) {
            dummyHead.next = head2;
        } else { // head2 == null
            dummyHead.next = head1;
        }
    }
}
```

## Reference
* [Reorder List [LeetCode]](https://leetcode.com/problems/reorder-list/description/)

{% include links.html %}
