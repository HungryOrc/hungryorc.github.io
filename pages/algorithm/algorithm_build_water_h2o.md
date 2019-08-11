---
title: "Building H2O"
tags: [algorithm, multi_thread]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_build_water_h2o.html
folder: algorithm
toc: false
---

## Description
There are two kinds of threads, oxygen and hydrogen. Your goal is to group these threads to form water molecules. There is a barrier where each thread has to wait until a complete molecule can be formed. Hydrogen and oxygen threads will be given releaseHydrogen and releaseOxygen methods respectively, which will allow them to pass the barrier. These threads should pass the barrier in groups of three, and they must be able to immediately bond with each other to form a water molecule. You must guarantee that all the threads from one molecule bond before any other threads from the next molecule do.

In other words:
* If an oxygen thread arrives at the barrier when no hydrogen threads are present, it has to wait for two hydrogen threads.
* If a hydrogen thread arrives at the barrier when no other threads are present, it has to wait for an oxygen thread and another hydrogen thread.
We don’t have to worry about matching the threads up explicitly; that is, the threads do not necessarily know which other threads they are paired up with. The key is just that threads pass the barrier in complete sets; thus, if we examine the sequence of threads that bond and divide them into groups of three, each group should contain one oxygen and two hydrogen threads.

Write synchronization code for oxygen and hydrogen molecules that enforces these constraints.

Note:
* Total length of input string will be 3n, where 1 ≤ n ≤ 20.
* Total number of H will be 2n in the input string.
* Total number of O will be n in the input string.

注意！这题是 **两个threads**
* 一个线程不停地收到让它打印 H 的命令，另一个线程不停地收到让它打印 O 的命令。收到命令不等于要立刻执行命令，可能执行，也可能先hold住不执行，那么后面还可能再来新的同样的命令，就积压在那里
* 对于每一个三元组来说，必须要能输出 HHO，HOH，OHH 这三种范式。如果总是输出比如 HHO 这样的话，是不可以的哦

我自己记一个java语法：
```java
// wake up all the threads that is waiting for this lock, including this thread
lock.notifyAll();
```

### Example
* Input: "OOHHHH"
  * Output: "HHOHHO", "HOHHHO", "OHHHHO", "HHOHOH", "HOHHOH", "OHHHOH", "HHOOHH", "HOHOHH" and "OHHOHH" are all valid answers.

## Solution

### Complexity
* Time: O(n) ？？？？
* Space: O(n) ？？？？

### Java
```java
class H2O {
    private Object lock;
    private int counter;
    private boolean oxy;
    
    public H2O() {
        lock = new Object();
        counter = 0;
        oxy = false;
    }

    // 这个函数的意思是 来了一个H，我们该怎么办老板
    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        synchronized (lock) {
            while (counter == 2) {
                if (oxy) {
                    reset();
                    break;
                }
                
                lock.wait();
            }
         
            // releaseHydrogen.run() outputs "H"
            releaseHydrogen.run();
            counter++;

            lock.notifyAll();
        }
    }

    // 这个函数的意思是，来了一个O，我们该怎么办老板
    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        synchronized (lock) {
            while (oxy) {
                if (counter == 2) {
                    reset();
                    break;
                }
                
                lock.wait();
            }
            
            // releaseOxygen.run() outputs "O"
            releaseOxygen.run();
            oxy = true;

            lock.notifyAll();
        }
    }
    
    private void reset() {
        oxy = false;
        counter = 0;
    }
}
```

## Reference
* [Building H2O [LeetCode]](https://leetcode.com/problems/building-h2o/description/)

{% include links.html %}
