---
title: "Pass by Value (值传递) in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_pass_by_value.html
folder: c++
toc: false
---

## Pass by Value

```c++
void passByValue(int n) {
    std::cout << "本函数操作的形参地址: " << &n // 这个地址是复制品的地址，和实参的地址是不一样的
    n ++; // 形参在这里 +1，但实参不会受到任何影响
}

int main()
{
    int n = 3;
    passByReference(n);
}
```

“值传递”所传递的，是实参的值的复本。所以对形参的任何操作都不会对实参造成任何影响。

* **存储**

  这个实参的值得复本当然也是要占用内存空间的。形参被作为被调函数的局部变量，即在stack里开辟内存空间以存放实参的值，从而成为实参的值的复本。







```c++
void multiplyTwo (int a, int b, int c)
{
  a *= 2; // a = 2, x = 1
  b *= 2; // b = 6, y = 3
  c *= 2; // c = 10, z = 5
}

int main ()
{
  int x = 1, y = 3, z = 5;
  multiplyTwo (x, y, z);
}
```


## Reference

* [C++ 值传递、指针传递、引用传递详解 [cnblogs.com]](http://www.cnblogs.com/yanlingyin/archive/2011/12/07/2278961.html)
* [Arguments passed by value and by reference [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/functions/)

{% include links.html %}