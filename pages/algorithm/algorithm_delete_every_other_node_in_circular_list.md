---
title: "Delete Every Other Node in a Circular Singly Linked List"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_delete_every_other_node_in_circular_list.html
folder: algorithm
toc: false
---

## Description
一个循环的 singly linked list，从head开始删，删除第1，3，5，7... 个node，只删一圈，不删第二圈。求剩下来的nodes，即要返回新的head node。
```java
class ListNode {
    int val;
    ListNode next;
}
```

### Example
* Input: 1->2->3->4->5(->1)
  * Output: 2->4(->2)
* Input: 1->2->3->4->5->6(->1)
  * Output: 2->4->6(->2)

## Solution：这题看起来简单，其实写起来好几个陷阱！要反复默写！

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public ListNode deleteEveyOtherNode(ListNode head) {
        // 注意，head的next不可能是null，其实这里任何node的next都不可能是null，因为是循环list
        if (head == null || head.next == head) {
            return null;
        }

        ListNode newHead = head.next;
        // 原本的head node 在后面再删！后面很巧妙
        
        ListNode cur = newHead;
        // 注意这里的while loop都不是判断next是否为null了，而是判断next是否为原本的head
        while (cur.next != head && cur.next.next != head) {
            cur.next = cur.next.next; // 删除cur.next
            cur = cur.next; // 挪动cur到原来的cur.next.nect
        }
        
        cur.next = newHead; // 关键在这里！一是把新的尾巴接到新的头，二是删除原来的head node！
        
        return newHead;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
