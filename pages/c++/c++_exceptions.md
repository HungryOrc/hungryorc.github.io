---
title: "Exceptions in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_exceptions.html
folder: c++
toc: false
---

## Exception

Exceptions react to exceptional circumstances such as runtime errors by **transferring control to special functions called "Handlers"**.

```c++
#include <iostream>
using namespace std;

int main () {
  try
  {
    throw 20;
  }
  catch (int e)
  {
    cout << "An exception occurred. Exception No." << e << '\n';
  }
  return 0;
}
```
The code under exception handling is enclosed in a `try` block. In the example above the code simply throws an exception `throw 20;`. A `throw` expression **accepts one parameter** (in this case the int value 20), which is passed as an argument to the "Exception Handler". The Exception Handler is declared with the `catch` keyword immediately after the closing brace of the `try` block. The syntax for catch is similar to a regular **function with one parameter**. The type of this parameter is very important, since the type of the argument passed by the throw expression is checked against it, and only in the case they **match**, the exception is caught by that handler.

Multiple handlers (i.e., `catch` expressions) can be chained, each one with a different parameter type. Only the handler whose argument type matches the type of the exception specified in the `throw` statement is executed. If an `...` is used as the parameter, that handler will catch all exceptions that had not been caught by other handlers:
```c++
try {
    // different kinds of throws
}
catch (int param1) { // statements }
catch (char param2) { // statements }
catch (...) { // statements } // 所谓的 ... 就是清理所有别人不要的杂碎
```

It's also possible to nest `try-catch` blocks within `try` blocks. In this case the internal `catch` block can forward the exception to the external level, this is done by the `throw` statement without argument in the `catch` block:
```c++
try {
    try {
        // do something
    } catch (int n) {
        throw; // <== 推卸责任到领导那里去
    }
} catch (...) {
    cout << "Hey there!";
}
```

## Standard Exceptions

The C++ Standard Library provides a base class specifically designed to declare objects to be thrown as exceptions. It is called `std::exception` and is defined in the `<exception>` header. This class has a `virtual` member function called what that returns a null-terminated character sequence (of type `char *`) and that can be overwritten in derived classes to contain some sort of description of the exception.

```c++
#include <iostream>
#include <exception>
using namespace std;

class myException: public exception
{
  // the "throw()" here means "all exceptions will call std::unexpected"
  virtual const char* what() const throw()
  {
    return "My exception happened";
  }
} myEx;

int main () {
  try
  {
    throw myEx;
  }
  catch (exception& e) // catches exception-type objects by reference
  {
    cout << e.what() << '\n'; // My exception happened.
  }
}
```

We have placed a handler that **catches exception-type objects by reference** (notice the `&` after `exception`), therefore this **also catches classes derived from exception**, like our `myEx` object of type `myException`.

All exceptions thrown by components of the C++ Standard Library throw exceptions derived from this exception class. These are:

| exception | description |
|:----------|:------------|
| `bad_alloc` | thrown by `new` on allocation failure
| `bad_cast` | thrown by `dynamic_cast` when it fails in a dynamic cast
| `bad_exception` | thrown by certain dynamic exception specifiers
| `bad_typeid` | thrown by `typeid`
| `bad_function_call` | thrown by empty `function` objects
| `bad_weak_ptr` | thrown by `shared_ptr` when passed a bad `weak_ptr`

Also deriving from `exception`, header `<exception>` defines two generic exception types that can be inherited by custom exceptions to report errors:

| exception | description |
|:----------|:------------|
| ` logic_error` | error related to the internal logic of the program
| `runtime_error` | error detected during runtime

A typical example where standard exceptions need to be checked for is on memory allocation:

```c++
// bad_alloc standard exception
#include <iostream>
#include <exception> // <==
using namespace std;

int main () {
  try {
    int * myarray = new int[1000];
  } catch (exception& e) {
    cout << "Standard exception: " << e.what() << endl;
  }
}
```

The exception that may be caught by the exception handler here is a `bad_alloc`. Because `bad_alloc` is derived from the standard base class `exception`, it can be caught (capturing by reference, captures all related classes).




## Reference

* [Exceptions [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/exceptions/)

{% include links.html %}