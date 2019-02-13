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
一个循环的 singly linked list，从head开始删，删除第1，3，5，7... 个node，只删一圈，不删第二圈。求剩下来的nodes。

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
    public void deleteNode(ListNode node) {
        // 如果 node.next 为空，即 node 是list里的最后一个node，
        // 则这一题做不了
        if (nodeToDel == null || nodeToDel.next == null) {
            return;
        }
        
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
