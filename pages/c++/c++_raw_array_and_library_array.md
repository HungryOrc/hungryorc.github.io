---
title: "Raw Array and Library Array in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_raw_array_and_library_array.html
folder: c++
toc: false
---

## Use Raw Arrays as Parameters in the Functions

In both C and C++, declaring a function to take an Array as parameter, in fact causes the function to take a Pointer (the Pointer to the 1st element of the Array). So the following 2 function signatures are identical:

```c++
// 以下2种方式是等价的：

// 直接传递数组的名字
void foo(int arr[]);
void foo(int arr[10]); // 在这种声明下，调用它的函数的实参(数组)的长度必须 >= 10

// 传递指向数组的指针
void foo(int *arr);
```

以上的做法都不是传递数组的 reference。下面才是正牌的传递数组的 reference 的方法。很绕。一般不必这么做。
```c++
// 传递数组的引用。这里 arr 是一个 有10个元素的整形数组 的引用
void foo(int (&arr)[10])
```

对于一般的变量比如 `int a` 来说，直接传递其名字就是 传递它的值的复本，并不会自动转化为 传递它的指针。
为什么数组就走特殊化呢？据说，是因为 C 的作者们觉得 pass array by value 过于昂贵所以将来也没有人会想这么做的 (勿预测未来)，
所以对于 array 就把 "传递array的名字" 自动转化为 "传递array的指针" 了，这样大家还可以少写一个 *，多欢乐。
第二个据说，C++ 也萧规曹随是为了 compatibility。所以这真的是个 nasty trick，很浪费本人的时间。


## Use "const" to Keep the Raw Arrays Intact

如果不想让函数修改传进来的 array，可以加个 const。下面几种方式都可以：

```c++
void foo(int const arr[]);
void foo(int const *arr);
void foo(int const (&arr)[10]);
```


## Library Arrays (Containers): "std::array", "std::vector"

The "Raw Arrays" like `int arr[]` are directly implemented as a language feature, inherited from the C language. They are a great feature, but by **restricting its copy** and **easily decay into pointers**, they suffer from an excess of optimization.

To overcome some of these issues with language built-in arrays, C++ provides an alternative array type as a standard "container". It is a type template (a class template, in fact) defined in header <array>. These containers operate in a similar way to built-in arrays, except that they allow being copied (an actually expensive operation that copies the entire block of memory) and decay into pointers only when explicitly told so (by means of its member data).

```c++
// example of std::array

#include <array>
using namespace std;

int main()
{
    array<int, 3> myArray = {1, 2, 3};

    for (int i = 0; i < myArray.size(); i++) {
        myArray[i] *= 2;
    }
}
```

Syntax differences in the example above:

* Declaration of the array
* Inclusion of an additional header library array
* The size of the Library Array is easy to be accessed


## Use Library Arrays as Parameters in the Functions

```c++
// use std::vector as parameter
void foo(std::vector<int>& v); // pass by reference of vector
```

当使用 container 时，也可以用 const 来避免程序对传进来的 array 的修改，比如：
```c++
void foo(std::vector<int>& v); // pass by const reference of vector
```


## Reference

* [Arrays](http://www.cplusplus.com/doc/tutorial/arrays/)
* [Passing Array Into Function - Pointer vs Reference (C++ vs C)](https://stackoverflow.com/questions/35531914/passing-array-into-function-pointer-vs-reference-c-vs-c)

{% include links.html %}