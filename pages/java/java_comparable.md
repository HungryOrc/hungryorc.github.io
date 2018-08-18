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


## Reference
* [Interface Comparable<T> [Oracle doc]](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)

{% include links.html %}
