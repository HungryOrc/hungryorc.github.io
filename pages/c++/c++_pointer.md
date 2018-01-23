---
title: "Pointer in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_pointer.html
folder: c++
toc: false
---

## The "Address-Of" Operator: &

```c++
addr = &var;
```
This will assign the **address** of the variable `var` to the variable `addr`, rather than the value (or content) of `var`. So if the value of `var` is 100 and the address of `var` is 20461005, then the value of `addr` will be 20461005. The address of `addr` is another question, we don't know it yet.

A variable that stores the address of another variable (like `addr` mentioned above) is called a Pointer. Pointers are said to "point to" the variable whose address they store. Pointer has many uses in lower level programming.

## The "Dereference" Operator: *

Pointers can be used to access the variable they point to directly, via the **Dereference Operator (*)**, this operator itself can be read as "value pointed to by (the pointer after this *)". So:
```c++
val = *addr; // `val` equal to the value pointed to by `addr`
```
Following the example in the previous section, this statement will assign the value 100 to the variable `val`. 综上所述：

```c++
var == 100 // true
&var == 20461005 // true

addr == 20461005 // true, since we had: addr = &var
*addr == 100 // true
*addr == var // true, 注意这是一个“compare by value”，并不是一个“compare by reference”

val == 100 // true, since we had: val = *addr
&val == 20461005 // false

// 注意区别！reference 的格式：
int& n1 = n2; // 意思是，变量n1 的 address 等于 变量n2 的 address，即 n1 是 n2 的 reference
```

注意：
* `*addr` 表示 指针变量`addr` 所指向的地址处所存的值。`*addr` 不等于 变量`var`，也不等于 变量`val`。
* 变量`var` 与 变量`val` 完全是两个事物。它们只是值相等而已，仅此而已。

## Declaring Pointers

```c++
int n1;
int * intPointer1 = &n1; // 合法。这是对整形指针的声明和赋值二合一，意思是：指针intPointer1 的 value 等于 变量n1 的 address

int * intPointer2;
intPointer = &n1; // 合法。这是先一句声明，再一句赋值

int * intPointer3;
*intPointer3 = &n1; // 瞎搞，这是概念混淆不清的产物
                    // *intPointer3 是取特定地址处所存的整形值，&n1 是取变量n1的地址。二者用=号相连即满足 牛头不对马嘴
```

The asterisk (*) here ONLY means that it is a pointer (it is part of its "type compound specifier"), that's all. 它和 "dereference operator" 一点关系都没有. They are simply two different things represented with the same sign. 所以这个 * 在这里没有任何“赋值”、“取地址”、“取值”...... 的意思，nothing!

Pointers can be initialized either to the address of a variable (such as in the case above), or to the value of another pointer (or array):

```c++
int n1;
int * intPointer1 = &n1;
int * intPointer2 = intPointer1; // 声明整形指针 intPointer2，并同时对它赋值————让它指向 intPointer1 所指向的地址
```


Due to the ability of a pointer to directly refer to the value that it points to, a pointer **has different properties** when it **points to different kinds of data types**. Once dereferenced, the type needs to be known. And for that, the declaration of a pointer needs to include the data type the pointer is going to point to. 

注意，This "type" is **NOT the type of the pointer itself**, but the **type of the data the pointer points to**. 比如下面3个Pointer变量，each of them points to a different data type, BUT, in fact, all of them are of the type `Pointer Object`, and all of them are likely to occupy the **same amount of space in memory** (the size in the memory of a pointer depends on the platform where the program runs). While, dramatically, the data to which they point to do not occupy the same amount of space nor are of the same data type.
```c++
int * intPointer;
char * charPointer;
double * decimalPointer;
```



```c++
#include <iostream>
using namespace std;

int main ()
{
  int n1, n2;
  int * myPointer; // 这个 * 在这里只是为了表示 myPointer 是一个 Pointer Object，仅此而已

  myPointer = &n1;
  *myPointer = 10;
  myPointer = &n2; // 改变一个指针所指向的变量
  *myPointer = 20;
  cout << n1 << endl; // 10
  cout << n2 << endl; // 20
  return 0;
}
```

## *p2 = *p1 与 p2 = p1

```c++
int * p1, * p2;

...
*p2 = *p1;  // the value pointed to by p2 = value pointed to by p1
p2 = p1;  // the address to which p2 is pointing = address to which p1 is pointing
```

## Use "const" to Prevent Changing the Value Pointed by the Pointer, or the Pointer Itself

```c++
int x, y = 10;
const int * intPointer = &y;
x = *intPointer; // 合法。将 intPinter 所指向的地址所存的值 赋给 x
*intPointer = x; // 不行。不能对 intPointer 所指向的地址所存的值 进行赋值
```

In the example above, a pointer to non-const variable (`y`) can be implicitly converted to a pointer to const (`const int * intPointer`). But pointers to const are not implicitly convertible to pointers to non-const.

One of the use cases of pointers to const elements is as function parameters: a function that takes a Pointer to a "const" variable as parameter cannot modify the value pointed by the Pointer:
```c++
void doSomething (const int* intPointer) 
{
    // cannot change the value pointed by intPointer, namely cannot change *intPointer
} 
```
In the example above, the value at the address to which `intPointer` was pointing cannot be modified, but the address to which `intPointer` was pointing can be modified! Namely `intPointer` can still be incremented or assigned a different address, although they still cannot modify the content they newly point to.

To make the Pointer point to a fixed address, i.e., to make the Pointer itself constant, just declare the Pointer by appending the `const` keyword after `*`, 下面是一个小结：
```c++
int x;
int * p1 = &x; // non-const Pointer, non-const int

const int * p2 = &x; // non-const Pointer, const int
int const * p3 = &x; // also, non-const Pointer, const int

int * const p4 = &x; // const Pointer, non-const int

const int * const p5 = &x; // const Pointer, const int 
```

## Pointers to Pointers

C++ allows Pointers that points to Pointers. The syntax simply requires an `*` for each level of indirection in the declaration of the Pointer:
```c++
char c1 = "z";
char * charPointer = &c1; // charPointer will be something like 20059
char ** charPointerPointer = &charPointer; // charPointerPointer will be something like 35447
```

* `charPointerPointer` is of type `char**`, its value is 35447
* `*charPointerPointer` is of type `char*`, its value is 20059
* `**charPointerPointer` is of type `char`, its value is 'z'


## Void Pointers

In C++, `void` represents **the Absence of Type**. So the Void Pointers are Pointers that **point to a value that has no type** (and thus also an **undetermined length** and **undetermined dereferencing properties**).

This gives void pointers a great flexibility, by being able to point to any data type. In exchange, they have a great limitation: the data pointed to by them **cannot be directly dereferenced** (which is logical, since we have no type to dereference to), and for that reason, any address in a void pointer needs to be transformed into some other pointer type that points to a concrete data type before being dereferenced.

One of its possible uses may be to pass generic parameters to a function:
```c++
#include <iostream>
using namespace std;

void increaseValue (void* voidPointer, int pointerSize)
{
  if ( pointerSize == sizeof(char) )
  { 
    char* charPointer = (char*) voidPointer; // (char*) 是类型转换，转换为 char pointer 类型
    (*charPointer) ++;
  }
  else if (pointerSize == sizeof(int))
  {
    int* intPointer = (int*) voidPointer; // (int*) 是类型转换，转换为 int pointer 类型
    (*intPointer) ++;
  }
}

int main ()
{
  char a = 'A';
  int b = 100;
  increaseValue (&a, sizeof(a));
  increaseValue (&b, sizeof(b));
}
```

## Invalid Pointers and Null Pointers

In C++, pointers are allowed to take any address value, no matter whether there actually is something at that address or not. Typical examples of this are **Uninitialized Pointers** and **Pointers to Nonexistence Elements of an Array**:
```c++
int * p1; // uninitialized pointer (local variable)

int intArray[10];
int * p2 = intArray + 10; // element out of bounds
``` 
But none of the above statements causes an error. What can cause an error is to dereference such a Pointer (i.e., actually accessing the value that the Pointer pointed to). Accessing such a pointer causes "undefined behavior", ranging from an error during runtime to accessing some random value.

But, sometimes, a pointer really needs **to explicitly point to nowhere**, and **not just an invalid address**. For such cases, there exists a special value that any pointer type can take: **the null pointer value**. This value can be expressed in C++ in two ways: either with an integer value of `0`, or with the `nullptr` keyword:
```c++
int * nullPointer1 = 0;
int * nullPointer2 = nullptr;
```
Here, both `nullPointer1` and `nullPointer2` are null pointers, meaning that they explicitly point to nowhere, and they both actually compare equal: all null pointers compare equal to other null pointers. 

It is also quite usual to see the defined constant `NULL` be used in older code to refer to the null pointer value, as shown below. `NULL` is defined in several headers of the standard library, and is defined as an alias of some null pointer constant value (such as `0` or `nullptr`).
```c++ 
int * nullPointer = NULL;
```

### Don't confuse "null pointers" with "void pointers"
* A null pointer is a value that any pointer can take, to represent that it is pointing to "nowhere".
* A void pointer is a type of pointer that can point to somewhere without a specific type.


## Pointers to Functions, Function as Argument to Another Function

C++ allows operations with pointers to functions. The typical use of this is for **passing a function as an argument to another function**. Pointers to functions are declared with the same syntax as a regular function declaration, except that the name of the function is enclosed between parentheses `()` and an `*` is inserted before the function name, so it will be something like `int (*functionName)`:
```c++
#include <iostream>
using namespace std;

int addition (int a, int b)
{
    return (a + b);
}

int subtraction (int a, int b)
{
    return (a - b);
}

// 注意，(*functionToBeCalled)(int, int) 这里的2个形参 只设定其类型都是int，不要设定形参的名字了！
int operation (int x, int y, int (*functionToBeCalled)(int, int))
{
  int result = (*functionToBeCalled)(x, y);
  return result;
}

int main ()
{
    int m, n;

    // `minus` is a Pointer to a function that has 2 parameters of type int
    // It is directly initialized to Point to the function `subtraction`
    // 注意这个声明的语法格式！
    int (*minus)(int, int) = subtraction;

    m = operation (7, 5, addition);
    n = operation (20, m, minus);
}
```


## Reference

* [Pointers](http://www.cplusplus.com/doc/tutorial/pointers/)

{% include links.html %}