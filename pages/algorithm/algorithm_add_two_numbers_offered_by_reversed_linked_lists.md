---
title: "Add Two Numbers, offered in two reversed linked lists"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_add_two_numbers_offered_by_reversed_linked_lists.html
folder: algorithm
toc: false
---

## Description
You are given two non-empty linked lists representing two non-negative integers. 
The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

给的2个数是反过来的，即小的位在list的前面，大的位在list的后面。要求的结果也要是反过来的，左边小位右边大位。

### Example
* Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
  * Output: 7 -> 0 -> 8, since 342 + 465 = 807，所以返回的结果也要求是**反过来的**

## Solution
略

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode prev = new ListNode(-1); // result里更大的位
        ListNode finalHead = prev;
        int carry = 0;
        
        // 关键的妙处：这个while里面的3个条件！
        // 把各种特殊情况，包括l1到头，l2到头，l1和l2都到头但是还再进了一位，
        // 都考虑到了！
        while (l1 != null || l2 != null || carry == 1) {
            ListNode cur = new ListNode(0); // result里更小的位
            
            int num1 = (l1 == null) ? 0 : l1.val;
            int num2 = (l2 == null) ? 0 : l2.val;
            int sum = num1 + num2 + carry;
            
            cur.val = sum % 10;
            carry = sum / 10;
            
            prev.next = cur;
            prev = cur;
            
            l1 = (l1 == null) ? null : l1.next;
            l2 = (l2 == null) ? null : l2.next;
        }
        
        return finalHead.next;
    }
}
```

如果要求给出的结果是正向的，即大位在左边，小位在右边，则这么写：
```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode prev = new ListNode(-1); // result里更大的位
        int carry = 0;

        while (l1 != null || l2 != null || carry == 1) {
            ListNode cur = new ListNode(0); // result里更小的位
            
            int num1 = (l1 == null) ? 0 : l1.val;
            int num2 = (l2 == null) ? 0 : l2.val;
            int sum = num1 + num2 + carry;
            
            cur.val = sum % 10;
            carry = sum / 10;
            
            cur.next = prev.next;
            prev.next = cur;
            
            l1 = (l1 == null) ? null : l1.next;
            l2 = (l2 == null) ? null : l2.next;
        }
        
        return prev.next;
    }
}
```

## Reference
* [Add Two Numbers [LeetCode]](https://leetcode.com/problems/add-two-numbers/description/)

{% include links.html %}
