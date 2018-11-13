---
title: "Partition Linked List into Smaller Half and Bigger Half"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_partition_linked_list_into_smaller_and_bigger_half.html
folder: algorithm
toc: false
---

## Description
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.
```java
 public class ListNode {
     int val;
     ListNode next;
     ListNode(int x) { val = x; }
 }
```

### Example
* Input: head = 1->4->3->2->5->2, x = 3
  * Output: 1->2->2->4->3->5

## Solution
搞两个list，一个串起来那些小的，一个串起来那些大的。最后两个连起来。速度前 1%

设两个dummy head，会方便很多

注意！最后别忘了把后半段list的最后一个node的next设为null！

### Complexity
* Time: O(n)
* Space: O(1)，没什么额外空间

### Java
```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        if (head == null) {
            return null;
        }
        
        // 小的编成一个list，大的编成一个list，最后再连在一起
        ListNode smallerDummyHead = new ListNode(-1);
        ListNode biggerAndEqualDummyHead = new ListNode(-1);
        
        ListNode curSmallerNode = smallerDummyHead;
        ListNode curBiggerAndEqualNode = biggerAndEqualDummyHead;
        
        while (head != null) {
            if (head.val < x) {
                curSmallerNode.next = head;
                curSmallerNode = head;
            } else { // >= x
                curBiggerAndEqualNode.next = head;
                curBiggerAndEqualNode = head;
            }
            head = head.next;
        }
        
        // concatenate the 2 lists into one list
        curSmallerNode.next = biggerAndEqualDummyHead.next;
        curBiggerAndEqualNode.next = null; // 别忘了这一步！
        
        return smallerDummyHead.next;
    }
}
```

## Reference
* [Partition List [LeetCode]](https://leetcode.com/problems/partition-list/description/)

{% include links.html %}
