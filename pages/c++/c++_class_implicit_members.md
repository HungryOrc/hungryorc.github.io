---
title: "Class: Implicit Members"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_class_implicit_members.html
folder: c++
toc: false
---

There are 6 kinds of special member functions that are implicitly declared on classes under certain circumstances. They are:
* Destructor
* Copy Constructor and Copy Assignment
* Move Constructor and Move Assignment

## Destructor

Destructors are member functions that do the necessary cleanup needed by a class when its lifetime ends. The classes we have defined in previous chapters **did not allocate any resource so they did not really require any clean up**. Destructor for an object is called at the end of the lifetime of the object.

Let's imageine that **a class allocates dynamic memory to store the string it had as data member**. In this case, it would be very useful to have a function called automatically at the end of the object's life in charge of releasing this memory. To do this, we use a destructor. A destructor is a member function very similar to a default constructor: it takes no arguments and returns nothing, not even void. It also uses the class name as its own name, but preceded with a "tilde sign" `~`:

```c++
#include <iostream>
#include <string>
using namespace std;#

class Example4 {
    string * ptr;
  public:
    // constructors:
    Example4 () : ptr(new string) {}
    Example4 (const string& str) : ptr(new string(str)) {}
    // destructor:
    ~Example4 () {delete ptr;} // 这个 destructor 做的是把 member data 的动态内存释放了
    // access content:
    const string& content() const {return *ptr;}
};

int main () {
  Example4 foo;
  Example4 bar ("Example");
  cout << "bar's content: " << bar.content() << '\n';
}
```

On construction, `Example4` allocates storage for a string, this storage is later released by the destructor. For objects `foo` and `bar` in the main function, their destructors are called at the end of the main function.


## Copy Constructor and Copy Assignment

### Copy Constructor

When an object (A) is passed an object (B) of the same class as argument, its **Copy Constructor** is invoked in order to construct a copy of B. 

"Copy Constructor" is a constructor whose 1st parameter is of **type reference** to the class itself (possibly `const` qualified) and which can be invoked with a single argument of this type. For example, for a class `MyClass`, the explicit Copy Constructor might have the following signature:
```c++
// Explicit Copy Constructor
MyClass::MyClass (const MyClass&); 
```

If a class has no custom "Copy Constructor/Assignment" nor "Move Constructor/Assignment" defined, an "IMPLICIT Copy Constructor" is automatically defined. For example, for a class such as:
```c++
class MyClass {
  public:
    int a, b;
    string c;
};
```
The Implicit Copy Constructor defined for this class will perform a **Shallow Copy** of the members of the class, roughly equivalent to:
```c++
// underlying form of the Implicit Copy Constructor, it will perform a Shallow Copy
MyClass::MyClass(const MyClass& x) : a(x.a), b(x.b), c(x.c) {}
```
But for classes that have Pointers as their members, performing a Shallow Copy means the value of the Pointer is copied, but the variable/object to which the Pointer was pointing is NOT. So the Pointer members of the copy object and the original object will point to the same object, and on destruction, both objects would try to delete the same block of memory, which will probably cause the program to crash at runtime. This can be solved by defining the custom Copy Constructor to perform a **Deep Copy**, like the following:

```c++
#include <iostream>
#include <string>
using namespace std;

class Example5 {
    string * ptr;
  public:
    // Constructor taking a "string&" type parameter
    Example5 (const string& str) : ptr(new string(str)) {}
    // Destructor
    ~Example5 () {delete ptr;}
    // Copy Constructor taking an "Example5&" type parameter, performs Deep Copy
    Example5 (const Example5& x) : ptr(new string(x.content())) {}
    
    const string& content() const {return *ptr;}
};

int main () {
  Example5 foo ("Example");

  // 下面这个并不是一个赋值语句！而是一个使用了 Copy Constructor 的 对象初始化！
  // 它相当于 Example5 bar (foo)
  Example5 bar = foo; 
  ...
}
```
The Deep Copy performed by this Copy Constructor allocates storage for a new string, which is initialized to contain a copy of the original object. In this way, both objects (copy and original) have distinct copies of the content stored in different locations.

### Copy Assignment

Objects are not only copied on Construction when they are initialized, they can also **be copied on any Assignment Operation**, 对比下面两段代码：

```c++
MyClass foo;
// 以下两句的意义完全相同，都是 Object initialization for "baz": Copy Constructor called
MyClass bar (foo); 
MyClass baz = foo;
```
`baz` is **initialized on construction USING AN EQUAL SIGN**. This is **just another syntax to CALL A SINGLE-ARGUMENT CONSTRUCTOR**. This is NOT an Assignment Operation, although it looks like one. This is NEITHER a Copy Assignment.        

```c++
MyClass foo;
foo = bar; 
```
Since Object `foo` has already been intialized, so `foo = bar` is an "Assignment Operation" which is calling a **"Copy Assignment"**. This Assignment Operation is not calling a "Copy Constructor".

The **"Copy Assignment Operator"** is a special function, it is an **Overload** of `operator=` which takes a `value` or `reference` of the class itself as parameter. The return value is generally a `reference` to `*this` (although this is not required). For example, for a class `MyClass`, the **"Copy Assignment"** might have the following signature:

```c++
MyClass& operator= (const MyClass&);
```

The "Copy Assignment Operator" is defined **implicitly** if a class has neither custom "Copy Assignment" nor custom "Move Assignment / Move Constructor". 

But again, the **Implicit** version of Copy Assignment Operator performs a **Shall Copy**. So in this case, for classes with Pointers as their member data, not only the classes incur the risk of deleting the same pointed object twice, but also **the Assignment creates Memory Leaks** by not deleting the object pointed by the object before the assignment. These issues can be solved by a Copy Assignment that **deleted the previous object** and performs a **Deep Copy**:

```c++
Example5& operator= (const Example5& x)
{
    delete ptr; // delete currently pointed string
    ptr = new string (x.content()); // allocate space for the new string, and do the copying
    return *this;
}
```

Or even better, since its string member is not constant, it could re-utilize the same string object:
```c++
Example5& operator= (const Example5& x)
{
    *ptr = x.content();
    return *this;
}
```

## Move Constructor and Move Assignment







## Reference

* [Special Members of Classes [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/classes2/)

{% include links.html %}