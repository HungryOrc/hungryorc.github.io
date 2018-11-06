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

Implement the following operations of a stack using queues.
* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* empty() -- Return whether the stack is empty.

Notice:
* You must use **only standard operations of a queue** -- which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
* Depending on your language, stack may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
* You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

### Example
```
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```

## Solution
诀窍有两个：
1. Queue可以poll出来元素以后再立刻offer给自己
2. 我们在每次push的时候，先push那个新元素进去，再把push之前queue里的所有元素倒置一次（新元素不动），那么就形成了 FILO 的格局！

过程举例：
* 放入1，形成 `(Head) 1 (Tail)`，这里的head和tail都是queue的head和tail，就是说offer是在tail，poll是在head
* 放入2，形成 `(Head) 1 2 (Tail)`
  * 把1 poll出去再offer回来，形成 `(Head) 2 1 (Tail)`。那么这个时候再poll的话，会先poll出来head的2，对于用户来说，就是后进的先出了，就实现了stack的行为方式
* ...
* 比如现在已经是 `(Head) 4 3 2 1 (Tail)`，放入5，那么会形成 `(Head) 4 3 2 1 5 (Tail)`
  * 按上面说的，倒置除了5以外的其他数，那么会得到 `(Head) 5 4 3 2 1 (Tail)`，again这就是我们要的stack的样子

这样的做法，每次push的时间消耗都是 O(n)，这恐怕无法再优化了。但只用一个queue来做的话，能在leetcode解答里速度排前 5%

### Complexity
* Time: 
  * Push: O(n)
  * Pop: O(1)
* Space: O(n)

### Java
```java
class MyStack {
    Queue<Integer> queue;
    
    public MyStack() {
        queue = new LinkedList<>();
    }

    public void push(int x) {
        queue.offer(x);
        
        // 关键！只倒置queue里的 n-1 个数！不倒置最新放进来的那个数！
        int size = queue.size();
        for (int i = 1; i <= size - 1; i++) {
            queue.offer(queue.poll());
        }
    }

    public int pop() {
        return queue.poll();
    }

    // peek
    public int top() {
        return queue.peek();
    }

    public boolean empty() {
        return queue.isEmpty();
    }
}
```

## Reference
* [Implement Stack using Queues [LeetCode]](https://leetcode.com/problems/implement-stack-using-queues/description/)

{% include links.html %}
