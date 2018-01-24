---
title: "Pass by Reference (引用传递) in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_pass_by_reference.html
folder: c++
toc: false
---

## Pass by Reference

```c++
void passByReference(int& n) {
    std::cout << "本函数操作的形参地址: " << &n // 这个地址和实参的地址是一致的
    n ++; // 形参在这里 +1，同时实参的值也会被 +1
}

int main()
{
    int n = 3;
    passByReference(n);
}
```



据说在底层的实现上，引用就是用指针来实现的。

“引用传递”所传递的，是实参的地址 **？？？？？**。对形参的操作就是对实参的操作。但“引用传递”和“指针传递”有本质上的不同。

* **存储**
  
  形参作为被调函数的局部变量在stack里开辟了内存空间，存放的是实参的地址 **？？？？？**。

  所以被调函数对形参的任何操作都被处理成“间接寻址”，即通过stack中存放的地址来访问主调函数中的实参。






## Pass by Reference and Avoid Changing the Original Variable: Use "const"

```c++
string concatenate (string a, string b)
{
  return a+b;
}
```

This function takes two strings as parameters (by value), and returns the result of concatenating them. By passing the arguments by value, the function forces a and b to be copies of the arguments passed to the function when it is called. And if these are long strings, it may mean copying large quantities of data just for the function call. But this copy can be avoided altogether if both parameters are made references:

```c++
string concatenate (string& a, string& b)
{
  return a+b;
}
```

Arguments by reference do not require a copy. The function operates directly on (aliases of) the strings passed as arguments, and, at most, it might mean the transfer of certain pointers to the function. In this regard, the version of concatenate taking references is more efficient than the version taking values, since it does not need to copy expensive-to-copy strings.

On the flip side, functions with reference parameters are generally perceived as functions that modify the arguments passed, because that is why reference parameters are actually for.

The solution is for the function to guarantee that its reference parameters are not going to be modified by this function. This can be done by qualifying the parameters as constant:

```c++
string concatenate (const string& a, const string& b)
{
  return a+b;
}
```

By qualifying them as const, the function is forbidden to modify the values of neither a nor b, but can actually access their values as references (aliases of the arguments), without having to make actual copies of the strings.

Therefore, const references provide functionality similar to passing arguments by value, but with an increased efficiency for parameters of large types. That is why they are extremely popular in C++ for arguments of compound types. Note though, that for most fundamental types, there is no noticeable difference in efficiency, and in some cases, const references may even be less efficient!



## Reference

* [C++ 中引用传递与指针传递的区别（进一步整理）[iteye.com]](http://xinklabi.iteye.com/blog/653643)
* [C++ 值传递、指针传递、引用传递详解 [cnblogs.com]](http://www.cnblogs.com/yanlingyin/archive/2011/12/07/2278961.html)
* [Arguments passed by value and by reference [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/functions/)

{% include links.html %}