---
title: "Class Inheritance and Interface in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_class_inheritance_interface.html
folder: java
toc: false
---

## Class Inheritance 

### Constructor of derived class
如果 super class 有一个或多个不带参数的 constructor，则 derived class 可以不定义任何 constructor，这样的话 derived class 相当于会默认使用 super class 的那些无参数的 constructor。如果 super class 还有别的有参数的 constructor(s)，那么那些 constructor 和 derived class 是没有关系的。

如果 super class 的所有 constructor 都是带参数的，那么 derived class 就必须：
* 定义自己的 constructor
* 而且这个 constructor 必须带参数
* 而且这个 constructor 里面必须含有 super class 的 constructor，用 `super` key word：
  ```java
  public class Animal {
      ...
      public Animal(String name, int age, boolean isPet) {
          ...
      }
      ...
  }
  ```
  ```java
  publis class Cat extends Animal {
      public Cat(String name, int age, boolean isPet) {
          super(name, age, isPet);
      }
      ...
  }
  ```
* derived class 的 constructor 的参数未必要与 super class 的 constructor 的参数一一对应,
  * 比如可以前者只take in 后者的部分参数，而后者的剩余参数在 call `super` 的时候在 `super` 里写定：
    ```java
    public class Cat extends Animal {
        public Cat(String name, int age) {
            super(name, age, true);
        }
    }
    ```
  * 也可以前者比后者的参数多：
    ```java
    public class Cat extends Animal {
        ...
        public Cat(String name, int age, boolean isPet, boolean catchRat) {
            super(name, age, isPet);
            this.catchRat = catchRat;
        }
    }
    ```

### Get the class name of the current object
This is a very useful trick, especially when you want to figure out the actual class of an object in a long long inheritance hierarchy:
```java
public class XYZ {
    ...
    public String getClassName() {
        return getClass().getSimpleName(); // this will return "XYZ"
    }
}
```
Since `getClass()` is a member function of the class `Object`, so any class in Java can use this method directly. 

### Abstract Class
Abstract Class 可以有 abstract method，也可以同时有 concrete method。


## Interface
Interface 是一个 contract 君子协定，接受了这个 interface 就意味着将来必须做这个 interface 所规定要做的所有事。
```java
// Action.java
public interface Action {
    void walk();
    String speak();
    double jump();
}
```
```java
// Cat.java
public class Cat implements Action {
    ...
    public void walk() {...}
    public String speak() {...}
    public int jump() {...}
    ...
}
```
注意！在 interface 里面不给methods任何 access modifier，比如 public、private 之类。在运用 interface 的class里需要这些 access modifier。

## Reference



{% include links.html %}
