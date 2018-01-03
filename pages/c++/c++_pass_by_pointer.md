---
title: "Pass by Pointer (指针传递) in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_pass_by_pointer.html
folder: c++
toc: false
---

## Pass by Pointer

```c++
void passByPointer(int* n) { // 这里的n是指针，*n是地址n处存储的值
    std::cout << "本函数操作的形参地址: " << n // 这个地址和实参的地址是一致的
    *n = *n + 1; // 形参 *n 在这里 +1，同时实参的值也会被 +1
}

int main()
{
    int n = 3;
    passByReference(&n);
}
```

“指针”本质上是一个**存放（变量的）地址的变量**，逻辑上它是一个独立的实体。
作为一个变量，指针自然是有自己的地址，注意：

* **指针变量自己的地址是不变的**
* **指针所存放的地址（或者称为指针所指向的地址）是可以改变的**
* **指针存放的地址中所存放的数据也是可以改变的**

具体来说，程序在编译时会将指针添加到“符号表”上，符号表上记录的是变量名和变量所对应的地址，指针在符号表上对应的的地址值就是指针变量自己的地址值，符号表生成以后就不会再改。

“指针传递”所传递的，是指向实参的地址的指针 **？？？？？**。对形参的操作就是对实参的操作。但“指针传递”和“引用传递”有本质上的不同。

* **存储**
  
  形参作为被调函数的局部变量在stack里开辟了内存空间，存放的是实参的地址 **？？？？？**。


* **类型安全**

  指针不是类型安全的，没有类型检查。

* **sizeof()**

  `sizeof(指针)` 是指针本身的大小。

* **++**
  
  `指针 ++` 的意义是：去到本 data type 的下一个存储单元




## Reference

* [C++ 中引用传递与指针传递的区别（进一步整理）](http://xinklabi.iteye.com/blog/653643)
* [C++ 值传递、指针传递、引用传递详解](http://www.cnblogs.com/yanlingyin/archive/2011/12/07/2278961.html)
* [Arguments passed by value and by reference](http://www.cplusplus.com/doc/tutorial/functions/)

{% include links.html %}