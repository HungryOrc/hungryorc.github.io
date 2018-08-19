---
title: "Comparable Interface in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_comparable.html
folder: java
toc: false
---

## Make a custom Class to be Comparable
Steps:
1. The custom Class must implement the `Interface Comparable`, and the type inside the `<...>` must be this Class's name.
2. Inside this Class, must override the `compareTo(...)` method

The following example sort the Cat Objects by their ages, in asccending order.
```java
public class Cat implements Comparable<Cat> {
    private int age;
    ...
    @Override
    public int compareTo(Cat other) {
        if (this.age > other.age) {
            return 1;
        } else if (this.age < other.age) {
            return -1;
        } else {
            reuturn 0;
        }
    }
}
```

Another example, reusing the `compareTo()` function of `String` type:
```java
public class Cat implements Comparable<Cat> {
    private String name;
    ...
    @Override
    public int compareTo(Cat other) {
        return this.name.compareTo(other.name); // this.name is a String
    }
}
```


## Reference
* [Interface Comparable<T> [Oracle doc]](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)

{% include links.html %}
