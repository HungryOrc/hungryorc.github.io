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
给一个 doubly linked list，其中每一个node都是`DoublyListNode` type 的。比如这个doubly linked list是：
```
1<->2<->3<->4<->5<->6<->7
```
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
* Time: O(n)，n 是后面给的要做grouping 的 nodes的个数，不是一开始的那个 doubly linked list的长度！那个更长
* Space: O(n)

### Java
代码很短。后面一大段是main函数。
```java
public class Solution {
    public int numOfGroups(DoublyListNode headNode, List<DoublyListNode> list) {
        if (headNode == null || list == null || list.size() == 0) {
            return 0;
        }

        Set<Integer> visited = new HashSet<>();
        int result = 0; // number of groups
        
        for (DoublyListNode node : list) {
            // if cur node is head or tail of the doubly linked list
            if (node.prev == null || node.next == null) { 
                int neighborVal = node.prev == null ? node.next.val : node.prev.val;
                if (!visited.contains(neighborVal)) {
                    result++;
                }
            // if cur node is an "internal" node in the doubly linked list
            } else { 
                int prevVal = node.prev.val;
                int nextVal = node.next.val;
                
                // 如果两边的邻居nodes都出现过，而且cur node就是唯一能连接两边这两个邻居nodes的node！
                // 所以这里result必须 -1，而且这里 -1 不会造成重复 -1，因为必然仅此一次！
                if (visited.contains(prevVal) && visited.contains(nextVal)) {
                    result--; 
                // 如果两边的邻居nodes都没出现过，那么这里result要 +1
                } else if (!visited.contains(prevVal) && !visited.contains(nextVal)) {
                    result++;
                }
            }
            
            visited.add(node.val); // 别忘了把cur node 放到visited里去
        }
        return result;
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {
        
        DoublyListNode n1 = new DoublyListNode(1);
        DoublyListNode n2 = new DoublyListNode(2);
        DoublyListNode n3 = new DoublyListNode(3);
        DoublyListNode n4 = new DoublyListNode(4);
        DoublyListNode n5 = new DoublyListNode(5);
        DoublyListNode n6 = new DoublyListNode(6);
        DoublyListNode n7 = new DoublyListNode(7);

        n1.next = n2;
        n2.prev = n1;
        n2.next = n3;
        n3.prev = n2;
        n3.next = n4;
        n4.prev = n3;
        n4.next = n5;
        n5.prev = n4;
        n5.next = n6;
        n6.prev = n5;
        n6.next = n7;
        n7.prev = n6;
        
        List<DoublyListNode> list = new ArrayList<>();
        list.add(n2);
        list.add(n4);
        list.add(n5);
        list.add(n7);
        
        Solution solu = new Solution();
        int result = solu.numOfGroups(n1, list);
        System.out.println(result);
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
