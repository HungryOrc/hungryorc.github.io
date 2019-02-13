---
title: "Group Doubly Linked List Nodes by Neighbors"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_group_doubly_linked_list_nodes_by_neighbors.html
folder: algorithm
toc: false
---

## Description
```java
class DoublyListNode {
    int val;
    DoublyListNode prev, next;
    
    public DoublyListNode(int val) {
        this.val = val;
        this.prev = null;
        this.next = null;
    }
}
```
给一个 doubly linked list，其中每一个node都是`DoublyListNode` type 的。比如这个doubly linked list是：`1<->2<->3<->4<->5<->6<->7`。

我们再给一些 nodes，这些nodes都是在前面那个 doubly linked list里面的（我们用 `List<DoublyListNode>` 的方式获得这些nodes）。
我们规定，每个node和它的邻居属于同一个group，但是这个“同属于一个group”的关系不能随意传播，只有
出现在上面的`List`里的nodes才能传播（这个规定也有他的道理，否则最初的那个 doubly linked list里的所有nodes都一定是同一个group的了），比如：
* 如果`list`里给了2和4，那么它们不是一个group，但如果给了2,3,4，则它们三个同属于一个group。
* 如果给了2,4,5,7，则2属于一个group，4和5属于一个group，7属于一个group。

问一共存在多少个group。

### Example
略

## Solution：我自己的方法，这题是周六mock Beiping的，写邮件问他这个解法对不对 ？？？

### Complexity
* Time: O(n)，n 是一开始的那个 doubly linked list的长度
* Space: O(n)

### Java
```java

```

## Reference
网上没找到这题

{% include links.html %}
