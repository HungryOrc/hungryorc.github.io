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
第一步，找mid node。第二步，reverse后半段。第三步，两个list merge到一起。这个方法的速度是前 1%，前提是像下面的代码这样全用iteration来做，
且在同一个函数里做完所有事。
如果用recursion或者用几个helper function来做（虽然那样的style更好），在leetcode上的速度测评会慢一些。

关键：别忘了把前半段的最后一个node的next设为null！不然会出现诡异错误！

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
        
        ListNode slow = head, fast = head;
        // 声明这个prevSlow是为了把前半段list的末尾设为null！
        ListNode prevSlow = null;
        
        // step 1
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            prevSlow = slow;
            slow = slow.next;
        }
        prevSlow.next = null;
        ListNode mid = slow;
        
        // step 2
        ListNode cur = mid;
        ListNode prev = null;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            
            prev = cur;
            cur = next;
        }
        ListNode newHeadOf2ndHalf = prev;
        
        // step 3
        ListNode n1 = head, n2 = newHeadOf2ndHalf;
        prev = new ListNode(Integer.MIN_VALUE);
        
        while (n1 != null && n2 != null) {
            ListNode nextN1 = n1.next;
            ListNode nextN2 = n2.next;

            prev.next = n1;
            n1.next = n2;
            
            prev = n2;
            n1 = nextN1;
            n2 = nextN2;
        }

        if (n1 == null && n2 != null) {
            n2.next = n1;
        } else if (n1 != null && n2 == null) {
            n1.next = n2;
        }
    }
}
```

## Reference
* [Reorder List [LeetCode]](https://leetcode.com/problems/reorder-list/description/)

{% include links.html %}
