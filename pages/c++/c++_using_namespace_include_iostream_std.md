---
title: "Using Namespace, #include, iostream, std (之后整理)"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_using_namespace_include_iostream_std.html
folder: c++
toc: false
---

## The std namespace

**All the entities (variables, types, constants, and functions)** of the standard C++ library are declared within the `std` namespace. `using namespace std` introduces direct visibility of all the names of the std namespace into the code.

Whether the elements in the std namespace are introduced with using declarations or are fully qualified on every use (like `std::cout << "Hi!";`) does NOT change the behavior or efficiency of the resulting program in ANY way. It is mostly a matter of style preference. For projects mixing libraries, explicit qualification tends to be preferred.


## Reference

* [Using Namespace and #include [stackoverflow.com]](https://stackoverflow.com/questions/5115556/c-using-namespace-and-include)
* [Relationship Between iostream.h and Namespace std [stackoverflow.com]](https://stackoverflow.com/questions/23589657/what-is-the-relationship-between-iostream-and-namespace-std)
* [The std namespace [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/namespaces/)

{% include links.html %}