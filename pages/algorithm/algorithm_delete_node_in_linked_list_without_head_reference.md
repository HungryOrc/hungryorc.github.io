---
title: "Delete a Node in Linked List with Only the Reference of This Node"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_delete_node_in_linked_list_without_head_reference.html
folder: algorithm
toc: false
---

## Description
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

只给一个参数，即要删除的node的reference，不给list的head node的reference！

Notice:
* The linked list will have at least two elements.
* All of the nodes' values will be unique.
* The given node will not be the tail and it will always be a valid node of the linked list.

### Example
* Input: remove the node with value == 1, the implicit list was: [4, 5, 1, 9], but we don't have the reference to 4
  * Output: the implicit result after the deletion shall be: [4, 5, 9], but we don't return anything in our function

## Solution
比较二逼的一道题。在此仅记录备查

### Complexity
* Time: O(1)
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
* [Delete Node in a Linked List [LeetCode]](https://leetcode.com/problems/delete-node-in-a-linked-list/description/)

{% include links.html %}
