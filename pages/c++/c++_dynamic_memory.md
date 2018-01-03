---
title: "Dynamic Memory in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_dynamic_memory.html
folder: c++
toc: false
---

C++ integrated the operators `new` and `delete` for the programs to allocate memory dynamically during runtime.

## Allocate Dynamic Memory using "new" and "new[]"

```c++
int * pointer1 = new int;

// In this case, the system will dynamically allocate space for 10 elements of type int,
// and return a pointer to the 1st element of the sequance (namely the block of memory allocated)
int * pointer2 = new int[10];
```

### Difference between declaring a normal array and allocating dynamic memory for a block of memory using "new"

The most important difference is that the size of a regular array needs to be a constant expression, and thus its size has to be determined at the moment of designing the program, before it is run. Dynamic memory allocation performed by `new` allows to assign memory during runtime using any variable value as the size.

Dynamic memory requested by the program is allocated by the system from the **memory heap**. However, computer memory is a limited resource, so there are no guarantees that all requests to dynamically allocate memory are going to be granted by the system. 

### Check if the allocation was successful

One way is to handle the exceptions. An exception of type `bad_alloc` is thrown when the allocation fails. 

Another way is `nothrow`. When a memory allocation fails, instead of throwing an exception and terminating the program, the Pointer returned by `new` is a "null pointer", thus the program continues its execution normally. This is done by using a special object called `nothrow`, declared in header `<new>`, as an argument for `new`:
```c++
int * p = new (nothrow) int[10];
```
`nothrow` is likely to produce less efficient code than exceptions, since it implies explicitly checking the pointer value returned after EVERY allocation.


## Release the Allocated Memory using "delete" and "delete[]"

In most cases, memory allocated dynamically is only needed during specific periods of time within a program; once it is no longer needed, it can be freed so that the memory becomes available again.
```c++
delete pointer1; // releases the memory of a single element allocated using "new"
delete[] pointer2; // releases the memory allocated for arrays of elements using "new" and a size in "[]"
```
The value passed as argument to `delete` shall be either a pointer to a memory block previously allocated with `new`, or a `null pointer` (in the case of a null pointer, delete produces no effect).

```c++
#include <iostream>
#include <new>
using namespace std;

int main ()
{
  int i,n;
  int * p;

  cout << "How many numbers would you like to type? ";
  cin >> i;
  // 注意，the value within brackets in the new statement is a variable i, not a constant
  p = new (nothrow) int[i];

  if (p == nullptr)
  {
    cout << "Error: memory could not be allocated";
  }
  else
  {
    for (n = 0; n < i; n++)
    {
      cout << "Enter number: ";
      cin >> p[n];
    }

    cout << "You have entered: ";
    for (n = 0; n < i; n++)
    {
      cout << p[n] << ", ";
    }

    delete[] p; // 删掉整一串
  }
}
```


## Reference

* [Dynamic Memory](http://www.cplusplus.com/doc/tutorial/dynamic/)

{% include links.html %}