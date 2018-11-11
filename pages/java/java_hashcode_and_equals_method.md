---
title: "Method 'hashCode' and 'equals' in Java"
tags: [java]
keywords: hashCode, equals
summary:
sidebar: mydoc_sidebar
permalink: java_hashcode_and_equals_method.html
folder: java
toc: false
---

如果不被重写（原生）的hashCode和equals是什么样的？
* 不被重写（原生）的hashCode值是根据内存地址换算出来的一个值。
* 不被重写（原生）的equals方法是严格判断一个对象是否相等的方法（object1 == object2）。

为什么需要重写equals和hashCode方法？
* 在我们的业务系统中判断对象时有时候需要的不是一种严格意义上的相等，而是一种业务上的对象相等。在这种情况下，原生的equals方法就不能满足我们的需求了
* 所以这个时候我们需要重写equals方法，来满足我们的业务系统上的需求。那么为什么在重写equals方法的时候需要重写hashCode方法呢？
      
我们先来看一下Object.hashCode的通用约定（摘自《Effective Java》第45页）
* 在一个应用程序执行期间，如果一个对象的equals方法做比较所用到的信息没有被修改的话，那么，对该对象调用hashCode方法多次，它必须始终如一地返回 同一个整数。在同一个应用程序的多次执行过程中，这个整数可以不同，即这个应用程序这次执行返回的整数与下一次执行返回的整数可以不一致。
* 如果两个对象根据equals(Object)方法是相等的，那么调用这两个对象中任一个对象的hashCode方法必须产生同样的整数结果。
* 如果两个对象根据equals(Object)方法是不相等的，那么调用这两个对象中任一个对象的hashCode方法，不要求必须产生不同的整数结果。然而，程序员应该意识到这样的事实，对于不相等的对象产生截然不同的整数结果，有可能提高散列表（hash table）的性能。



看下面这两篇文章，并且列入最后的ref列表里：

https://blog.csdn.net/zknxx/article/details/53862572

https://zhuanlan.zhihu.com/p/19655110


## Reference

{% include links.html %}
