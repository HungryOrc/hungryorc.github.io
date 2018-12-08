---
title: "Add Two Numbers, offered in two linked lists"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_add_two_numbers_offered_by_linked_lists.html
folder: algorithm
toc: false
---

## Description
You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

* 给的2个数，大的位在list的前面，小的位在list的后面
* 要求的结果也要是左边大位，右边小位

### Follow up
What if you **cannot modify the input lists**? In other words, **reversing the lists is not allowed**.

### Example
* Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
  * Output: 7 -> 8 -> 0 -> 7, since 7243 + 564 = 7807

## Solution：用2个Stack来做，速度 前1%
用2个stacks来装lists里的数，这样就把各个数位反过来了，个位最先被处理。这种方法没有把lists反转过来，符合follow up question的要求

### Complexity
* Time: O(n)
* Space: O(n)，两个stacks

### Java
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Deque<Integer> s1 = new ArrayDeque<>();
        Deque<Integer> s2 = new ArrayDeque<>();
        
        while (l1 != null) {
            s1.push(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            s2.push(l2.val);
            l2 = l2.next;
        }
        
        int carry = 0;
        ListNode prev = new ListNode(-1);
    
        // 关键的妙处：这个while里面的3个条件！
        // 把各种特殊情况都考虑到了！
        // 包括s1到头，s2到头，s1和s2都到头但是还再进了一位（比如最后是 5 + 9 = 14）
        while (!s1.isEmpty() || !s2.isEmpty() || carry == 1) {
            ListNode cur = new ListNode(0);
            
            int sum = carry;
            if (!s1.isEmpty() && !s2.isEmpty()) {
                sum += s1.pop() + s2.pop();
            } else if (s1.isEmpty() && !s2.isEmpty()) {
                sum += s2.pop();
            } else if (s2.isEmpty() && !s1.isEmpty()) {
                sum += s1.pop();
            }
        
            cur.val = sum % 10;
            carry = sum / 10;
            
            // 另一个关键的妙处：下面这两句这么做，就能保证，新来的node在左边，之前来的node在右边；
            // 而不是像一般的linkedlist那样，新来的在右边老的在左边
            cur.next = prev.next;
            prev.next = cur;
        }
        
        return prev.next;
    }
}
```

## Reference
* [Add Two Numbers II [LeetCode]](https://leetcode.com/problems/add-two-numbers-ii/description/)

{% include links.html %}
