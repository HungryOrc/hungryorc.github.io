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
### Use 2 Stacks
最直观来说，用2个Stack尾对尾（栈底贴栈底，两个栈顶口向外）就可以实现，如下图：
```
             left Stack           right Stack
     <-- topL        bottomL    bottomR    topR -->
          7    6    5    4   ||   3    2    1  
              
   <-- dequeue Head                   dequeue Tail -->
```
这样的设计，push都没问题，pop的时候只要上面2个Stack不空也没问题。
问题就是如果pop的时候被pop的那一端的那个stack已经空了，比如pop head，结果left stack已经空了，
就得从right stack把所有元素倒到left stack，再pop掉最上面那个元素，再把剩下的元素都倒回到right stack去。
这样，一头空了以后，对这一头的每一次的pop都一定是n的时间复杂度，太慢了。

### Use 3 Stacks
改进方式是，再加上第三个stack做buffer。每次一头空了以后，就从另一头把一半的元素挪过来。关键是要保持“两头”的相对顺序。如下图：
```
         left Stack    right Stack      buffer Stack
       7 6 5 4 3 2 1 || empty       
   =>          3 2 1 || empty            4 5 6 7 ||
   =>          empty || 3 2 1            4 5 6 7 ||
   =>        7 6 5 4 || 3 2 1          
```
这样的话，就算一头空了以后，每次在这一头的pop，amortized time cost也还是 O(1)

### Complexity，用3个stacks
* Time: 
  * Push: O(1)
  * Pop: armortize O(1)
* Space: O(n)

### Java
代码虽然长，其实很简明，要注意的只有一个helper function
```java
public class DequeueByStacks {
    private Deque<Integer> headStack;
    private Deque<Integer> tailStack;
    private Deque<Integer> bufferStack;
	
    public DequeueByStacks() {
        this.headStack = new ArrayDeque<>();
        this.tailStack = new ArrayDeque<>();
        this.bufferStack = new ArrayDeque<>();
    } 
			
    public void pushHead(int num) {
        headStack.push(num);
    }

    public void pushTail(int num) {
        tailStack.push(num);
    }
	
    private boolean completelyEmpty() {
        return headStack.isEmpty() && tailStack.isEmpty();
    }
	
    public int popHead() {
        if (this.completelyEmpty()) {
            return Integer.MIN_VALUE;
        } else if (!headStack.isEmpty()) {
            return headStack.pop();
        } else {
            dumpHalfAndMaintainSequence(tailStack, headStack);
            return headStack.pop();
        }
    }
	
    // 这个函数与 popHead 是对称的
    public int popTail() {
        if (this.completelyEmpty()) {
            return Integer.MIN_VALUE;
        } else if (!tailStack.isEmpty()) {
            return tailStack.pop();
        } else {
            dumpHalfAndMaintainSequence(headStack, tailStack);
            return tailStack.pop();
        }
    }
	
    // did not implement peek() here, 
    // since in this question, peek() is almost the same procedure as pop()
	
    // 最关键的函数在此！
    private void dumpHalfAndMaintainSequence(Deque<Integer> fromStack, Deque<Integer> toStack) {
	int halfSize = fromStack.size() / 2;
        
	dump(fromStack, this.bufferStack, halfSize);
	dump(fromStack, toStack, fromStack.size());
	dump(this.bufferStack, fromStack, halfSize);
    }
	
    private void dump(Deque<Integer> fromStack, Deque<Integer> toStack, int dumpSize) {
        for (int i = 1; i <= dumpSize; i++) {
            toStack.push(fromStack.pop());
        }
    }

    // main
    // -----------------------------------------------------------------
    public static void main(String[] args) {
	DequeueByStacks myDequeue = new DequeueByStacks();

	myDequeue.pushHead(3); // 3 ||
	myDequeue.pushHead(2); // 2 3 ||
	myDequeue.pushHead(1); // 1 2 3 ||
	myDequeue.pushTail(4); // 1 2 3 || 4
	myDequeue.pushTail(5); // 1 2 3 || 4 5

	System.out.println(myDequeue.popTail()); // 1 2 3 || 4, return 5
	System.out.println(myDequeue.popTail()); // 1 2 3 || , return 4
	System.out.println(myDequeue.popTail()); // 1 || 2, return 3
	System.out.println(myDequeue.popTail()); // 1 || , return 2
	System.out.println(myDequeue.popTail()); // || , return 1
	System.out.println(myDequeue.popTail()); // || , return -1
    }
}
```

## Reference
网上没看见这题

{% include links.html %}
