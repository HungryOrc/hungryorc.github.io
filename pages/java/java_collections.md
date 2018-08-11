---
title: "Collections in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_collections.html
folder: java
toc: false
---

## Interfaces for the concrete Collection Classes
Currently there are 4 Interfaces for the implementation of the concrete Collection Classes in Java: `List`, `Deque` (sounds "Dee-Q"), `Set`, `Map`. 
The interface `Queue` is not quite used now. Some concrete Collection Classes implements more than 1 type of the above Interfaces.

| Interface | Linked List | Resizable Array | Balanced Tree | Hash Table | Hash Table + Linked List |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| List | LinkedList | ArrayList | | | |
| Deque | LinkedList | ArrayDeque | | | |
| Set | | | TreeSet | HashSet | LinkedHashSet |
| Map | | | TreeMap | HashMap | LinkedHashMap |

When declaring a Collection object in Java, it's a good practice to set the type of the collection as the Interface, and when instantiating the object, use a construction method for a concrete Collection class. Such as:
```java
List<String> myList = new ArrayList<>();
```

## Reference
* [Collection Implementations [Oracle]](https://docs.oracle.com/javase/tutorial/collections/implementations/index.html)


{% include links.html %}
