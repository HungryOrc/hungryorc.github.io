---
title: "Implement Deque with Stacks"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_implement_deque_with_stacks.html
folder: algorithm
toc: false
---

## Description
这题是开放性的，没有要求用几个stacks

### Example
略

## Solution with 2 or 3 Stacks
最直观来说，用2个Stack尾对尾（栈底贴栈底，两个栈顶口向外）就可以实现，如下图：

        leftStack          rightStack
    topL      bottomL   bottomR    topR
    7    6    5    4  ||  3    2    1  
dequeueHead                        dequeueTail

这样的设计，push都没问题，pop的时候只要上面2个Stack不空也没问题；
问题就是如果pop的时候被pop的那一端的那个stack已经空了，比如pop head，结果left stack已经空了，
就得从另一个stack即right stack把所有元素倒到left stack，再pop掉最上面那个元素，再把剩下的元素都倒回到right stack去。
这样，一头空了以后，对这一头的每一次的pop都一定是n的时间复杂度，太慢了。

改进方式是，再加上第三个stack做buffer。每次一头空了以后，就从另一头把一半的元素挪过来。关键是要保持“两头”的相对顺序。如下图：

         leftStack      rightStack      bufferStack
       7 6 5 4 3 2 1 || empty       
   =>          3 2 1 || empty            4 5 6 7 ||
   =>          empty || 3 2 1            4 5 6 7 ||
   =>        7 6 5 4 || 3 2 1          
   
这样的话，就算一头空了以后，每次在这一头的pop，amortized time cost也还是 O(1)

### Complexity，用3个stacks
* Time: 
  * Push: O(1)
  * Pop: worst case O(n), armortize O(1)
* Space: O(n)

### Java
```java

```

## Reference
网上没看见这题

{% include links.html %}
