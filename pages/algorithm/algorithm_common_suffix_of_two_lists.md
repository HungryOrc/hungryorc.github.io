---
title: "Common Suffix (后缀) of 2 Singly Linked Lists"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_common_suffix_of_two_lists.html
folder: algorithm
toc: false
---

## Description：Google Onsite 题
```java
class ListNode {
    char val;
    ListNode next;
    
    public ListNode(char c, ListNode n) {
        this.val = c;
        this.next = n;
    }
}
```

### Example
* Input: a->b->c->b->c->d, e->b->c->d
  * Output: 3，即 最后的 b->c->d 那一段

## Solution 1：把2个list都翻转过来，然后从头到尾看有多长的相同段

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int longestCommonSuffix(ListNode head1, ListNode head2) {
        ListNode tail1 = reverseList(head1);
        ListNode tail2 = reverseList(head2);
        
        int result = 0;
        while (tail1 != null && tail2 != null && tail1.val == tail2.val) {
            result++;
            tail1 = tail1.next; // 这里的 next node其实就是反转前的previous node
            tail2 = tail2.next;
        }
        return result;
    }

    private ListNode reverseList(ListNode node) {
        if (node == null || node.next == null) {
            return node;
        }
        
        ListNode nextNode = node.next;
        ListNode newHead = reverseList(nextNode);
        
        nextNode.next = node;
        node.next = null;
        
        return newHead;
    }
}
```

## Solution 2：把较长的那个list向前走几步，达到和较短的那个一样的长度
比如要比较：
```
a->b->c->b->c->d
e->b->c->d
```
那么就把长的那个的head node 往后走几步：
```
a->b->c->b->c->d
      ^
      e->b->c->d
```
这样就不用反转list。从长list的新head开始，对比短list的head。二者同步向右走。遇到相同的nodes，则什么也不用做。
遇到不同的nodes，则 length of common suffix 就要减掉从新head到当前的有差别的node的这一段。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int longestCommonSuffix(ListNode head1, ListNode head2) {
        ListNode tail1 = reverseList(head1);
        ListNode tail2 = reverseList(head2);
        
        int result = 0;
        while (tail1 != null && tail2 != null && tail1.val == tail2.val) {
            result++;
            tail1 = tail1.next; // 这里的 next node其实就是反转前的previous node
            tail2 = tail2.next;
        }
        return result;
    }

    private ListNode reverseList(ListNode node) {
        if (node == null || node.next == null) {
            return node;
        }
        
        ListNode nextNode = node.next;
        ListNode newHead = reverseList(nextNode);
        
        nextNode.next = node;
        node.next = null;
        
        return newHead;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
