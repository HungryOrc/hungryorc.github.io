---
title: "Class Inheritance in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_class_inheritance.html
folder: java
toc: false
---

## Constructor of derived class
如果 super class 有一个或多个不带参数的 constructor，则 derived class 可以不定义任何 constructor，这样的话 derived class 相当于会默认使用 super class 的那些无参数的 constructor。如果 super class 还有别的有参数的 constructor(s)，那么那些 constructor 和 derived class 是没有关系的。

如果 super class 的所有 constructor 都是带参数的，那么 derived class 就必须：
* 定义自己的 constructor
* 而且这个 constructor 必须带参数，这些参数一一对应于 super class 的 constructor 的那些参数
* 而且这个 constructor 里面必须含有 super class 的 constructor，用 `super` key word：
  ```java
  public class Animal {
      String name;
      int age;
      public Animal(String name, int age) {
          this.name = name;
          this.age = age;
      }
      ...
  }
  ```
  ```java
  publis class Cat extends Animal {
      public Cat(String name, int age) {
          super(name, age);
      }
      ...
  }
  ```

## Reference



{% include links.html %}
