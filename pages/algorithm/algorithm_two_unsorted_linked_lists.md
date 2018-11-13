---
title: "Merge Two Unsorted Linked Lists"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_two_unsorted_linked_lists.html
folder: algorithm
toc: false
---

## Description
直接把2个linked list 合并到一起，每次每边出一个node，不管大小

这一题很简单，网上并没有，列在这里只是为了备查。因为它可能会是别的更复杂的题目里的一个helper function

### Example
略

## Solution
略

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public void mergeLists(ListNode head1, ListNode head2) {
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
```

## Reference
这题太简单了，网上没有

{% include links.html %}
