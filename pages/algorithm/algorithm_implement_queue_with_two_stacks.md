---
title: "Implement Queue with 2 Stacks"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_implement_queue_with_two_stacks.html
folder: algorithm
toc: false
---

## Description
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
用 stack1 存push进来的东西，用 stack2 存将要（按queue的规则）pop出去的东西. 两个stack之间没有重复存的东西

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
