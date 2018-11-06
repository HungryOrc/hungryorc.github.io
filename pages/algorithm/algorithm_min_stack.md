---
title: "Implement a kind of Stack that can Get Min"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_min_stack.html
folder: algorithm
toc: false
---

## Description
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element, 相当于 peek()
* getMin() -- Retrieve the minimum element in the stack.

### Example
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

## Solution 1: 用2个Stack，一个记数，一个记当前min以及此min的有效范围
这个方法不容易想到。2个Stack如下:
* Stack1 里装的就是各个数，没啥稀奇。要找min需要借助 Stack2
* Element in Stack2 = `<minValue,  从Stack1的size为多少开始,Stack1的min就是这个minValue了>`
```
   Stack1  ||   3        1       2       -6       -8      -8     10   ...
   Stack2  || <3,1>    <1,2>           <-6,4>   <-8,5>                ...
```


### Complexity
* Time:
  * Push: O(1)
  * Pop and Peek: O(1)
* Space: O(n)

### Java
代码看起来长，其实逻辑简明
```java
// helper class
class MinAndSize {
    int min, size;
    public MinAndSize(int min, int size) {
        this.min = min;
        this.size = size;
    }
}

public class MinStack {
    Deque<Integer> stack;
    Deque<MinAndSize> minAndSizes;
        
    public MinStack() {
        stack = new ArrayDeque<>();
        minAndSizes = new ArrayDeque<>();
    }
    
    public void push(int x) {
        stack.push(x);
        
        if (minAndSizes.size() == 0) {
            minAndSizes.push(new MinAndSize(x, 1));
        } else {
            if (x < minAndSizes.peek().min) {
                this.minAndSizes.push(
                    new MinAndSize(x, stack.size()));
            } 
        }
    }
    
    public void pop() {
        if (stack.isEmpty()) {
            return;
        }
        
        if (minAndSizes.peek().size == stack.size()) {
            minAndSizes.pop();
        }
        
        stack.pop();
    }
    
    // peek
    public int top() {
        if (stack.isEmpty()) {
            return Integer.MIN_VALUE;
        }

        return stack.peek();        
    }
    
    public int getMin() {
        if (stack.isEmpty()) {
            return Integer.MIN_VALUE;
        }

        return minAndSizes.peek().min;         
    }
}
```

## Solution 2: 在每个数下面，垫一个之前的min值
关键在于：每一个push要push两个东西！先push进来本number到来之前这个Stack里的min值，再push本number

这个方法在LeetCode的测评速度比前一个快一些
        
### Complexity
* Time:
  * Push: O(1)
  * Pop and Peek: O(1)
* Space: O(n)

### Java
```java
public class MinStack {
    int minValue;
    Deque<Integer> stack;
    
    public MinStack() {
        this.minValue = Integer.MAX_VALUE;
        this.stack = new ArrayDeque<>();
    }

    public void push(int number) {
        // 先push进来本number到来之前这个Stack里的min值，再push本number
        stack.push(this.minValue); 
        stack.push(number);
        
        // 最后再更新min值
        minValue = Math.min(minValue, number);
    }

    public int pop() {
        int poppedValue = stack.pop();
        int prevMin = stack.pop();
        
        minValue = prevMin;
        return poppedValue;
    }

    public int top() { // peek
        return stack.peek();
    }
    
    public int getMin() {
        return minValue;
    }
}
```

## Reference
* [Min Stack [LeetCode]](https://leetcode.com/problems/min-stack/description/)

{% include links.html %}
