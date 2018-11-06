---
title: "Implement Stack with Queues"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_implement_stack_with_queues.html
folder: algorithm
toc: false
---

## Description
这一题是开放性的，没有要求用几个Queue。直觉上应该至少用2个Queue才行，但其实用一个就可以了！而且速度是最快的！见代码

Implement the following operations of a queue using stacks.
* push(x) -- Push element x to the back of queue.
* pop() -- Removes the element from in front of queue.
* peek() -- Get the front element.
* empty() -- Return whether the queue is empty.

Notice:
* You must use **only standard operations of a stack** -- which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
* Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
* You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

### Example
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

## Solution
诀窍有两个：
1. Queue可以poll出来元素以后再立刻offer给自己
2. 我们在每次push的时候，先push那个新元素进去，再把push之前queue里的所有元素倒置一次（新元素不动），那么就形成了 FILO 的格局！


### Complexity
* Time: 
  * Push: O(1)
  * Pop: worst case O(n), armortize O(1)
* Space: O(n)

### Java
```java
class MyQueue {
    Deque<Integer> pushStack;
    Deque<Integer> popStack;

    public MyQueue() {
        pushStack = new ArrayDeque<>();
        popStack = new ArrayDeque<>();
    }

    public void push(int x) {
        pushStack.push(x);
    }
    
    // 注意这个helper function
    private void dumpAll(Deque<Integer> fromStack, Deque<Integer> toStack) {
        while (!fromStack.isEmpty()) {
            toStack.push(fromStack.pop());
        }
    }

    public int pop() {
        if (popStack.isEmpty()) {
            dumpAll(pushStack, popStack);
        }
        return popStack.pop();
    }

    public int peek() {
        if (popStack.isEmpty()) {
            dumpAll(pushStack, popStack);
        }
        return popStack.peek();
    }

    public boolean empty() {
        return popStack.isEmpty() && pushStack.isEmpty();
    }
}
```

## Reference
* [Implement Queue using Stacks [LeetCode]](https://leetcode.com/problems/implement-queue-using-stacks/description/)

{% include links.html %}
