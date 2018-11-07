---
title: "Reverse Singly Linked List"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_reverse_singly_linked_list.html
folder: algorithm
toc: false
---

## Description
Reverse a singly linked list. 
* Do it both iteratively and recursively
* Definition for singly-linked list.
  ```
  public class ListNode {
      int val;
      ListNode next;
      ListNode(int x) { val = x; }
  }
  ```

### Example
* Input: `1->2->3->4->5->NULL`
  * Output: `5->4->3->2->1->NULL`

## Solution 1: Recursion, with a helper function

### Complexity
* Time: O(n)
* Space: O(n)，因为有n层call stack

### Java
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        
        return reverse(null, head, head.next);
    }
    
    private ListNode reverse(ListNode prev, ListNode cur, ListNode next) {
        cur.next = prev;
        
        if (next == null) {
            return cur;
        }
        
        return reverse(cur, next, next.next);
    }
}
```

## Solution 2: Iteration

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        
        ListNode cur = head;
        ListNode prev = null;
        
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            
            prev = cur;
            cur = next;
        }
        
        return prev;
    }
}
```

## Solution 3: Recursion without helper function

### Complexity
* Time: O(n)
* Space: O(n)，因为有n层call stack

### Java
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null)
            return head;
        
        // 第一步：不管当前node，它以后的nodes会发生什么
        ListNode nextNode = head.next;
        ListNode newHead = reverseList(nextNode);   
     
        // 第二步：经过处理后的后部，与当前的前部，应该发生哪些关系？一个也不要漏！
        nextNode.next = head;
        
        /* 特别注意下面这句 ！！！
          如果是第一个head node，这么做就完事儿了。它也就该next -> null 
          但是对于原head以后的任何nodes，把它们的next设为null怎么行呢？！！窍门在于：
          reverseList这个函数在上面是不断嵌套自己的，
          在里面的一层，把某个node的next设为null以后；在紧邻着的外面的一层，就会再把它的next设为它之前的prev 
         
          从另外一个角度理解这个问题：对于head来说，sub problem是：
            1 -> 2 -> 3 -> 4 -> null
                \___sub problem ___/
              
          sub problem搞定以后，它的结果其实是这个样子：注意！节点2 在此时是指向null的！
            1 -> 2 <- 3 <- 4
                 |
                 V
                null                                                                       */
     
        head.next = null;
     
        return newHead;
    }
}
```

## Reference
* [Reverse Linked List [LeetCode]](https://leetcode.com/problems/reverse-linked-list/description/)

{% include links.html %}
