---
title: "Type Conversions in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_type_conversion.html
folder: c++
toc: false
---

## Implicit Conversion

### Standard Conversion

Implicit conversions are automatically performed when a value is copied to a compatible type. For example:
```c++
short a = 2000;
int b;
b = a; // the value of a is promoted from short to int, implicitly
```

The example above is a "Standard Conversion". Standard Conversion affect fundamental data types, and allow the conversions between numerical types (short to int, int to float, double to int...), to or from bool, and some pointer conversions.

#### Standard Conversion for Fundamental Data Types

Converting to int from some smaller integer type, or to double from float is known as promotion, and is guaranteed to produce the exact same value in the destination type. Other conversions between arithmetic types may not always be able to represent the same value exactly:

* If a negative integer value is converted to an unsigned type, the resulting value corresponds to its 2's complement bitwise representation (i.e., -1 becomes the largest value representable by the type, -2 the second largest, ...).

* The conversions from/to bool consider false equivalent to zero (for numeric types) and to null pointer (for pointer types); true is equivalent to all other values and is converted to the equivalent of 1.

* If the conversion is from a floating-point type to an integer type, the value is truncated (the decimal part is removed). If the result lies outside the range of representable values by the type, the conversion causes undefined behavior.

* Otherwise, if the conversion is between numeric types of the same kind (integer-to-integer or floating-to-floating), the conversion is valid, but the value is implementation-specific (and may not be portable).

Some of these conversions may imply a loss of precision, which the compiler can signal with a warning. This warning can be avoided with an explicit conversion.

#### Standard Conversion for Non-standard Data Types

For non-fundamental types, **Arrays and Functions are implicitly converted to Pointers**, and Pointers in general allow the following conversions:

* Null pointers can be converted to pointers of any type

* Pointers to any type can be converted to void Pointers.

* Pointer upcast: Pointers to a derived class can be converted to a Pointer of an accessible and unambiguous base class, without modifying its const or volatile qualification.

### Implicit Conversions with Classes

* **Single Argument Constructors**: Allow implicit conversion from a particular type to initialize an object

* **Assignment Operator**: Allow implicit conversion from a particular type on assignments

* **Type Case Operator**: Allow implicit conversion to a particular type

```c++
#include <iostream>
using namespace std;

class A {};

class B {
  public:
    // conversion from A (constructor):
    B (const A& x) {}

    // conversion from A (assignment):
    B& operator = (const A& x) { return *this; }

    // conversion to A (type-cast operator)
    operator A() { return A(); }
    // The type-cast operator uses a particular syntax: 
    // it uses the `operator` keyword followed by the destination type and an empty set of parentheses. 
    // Notice that the return type is the destination type and thus is not specified before the `operator` keyword.
};

int main ()
{
  A foo;
  B bar = foo;    // calls constructor
  bar = foo;      // calls assignment
  foo = bar;      // calls type-cast operator
}
```

### Keyword "explicit"

On a function call, C++ **allows one implicit conversion to happen for each argument**. This may be somewhat problematic for classes. For example, if we add `void fn (B arg) {}` to the last example, the function takes an argument of type `B`, but it could also be called with an object of type `A`, like: `fn (foo)` (we had defined previously: `A foo;`). This might not be what was intended. This can be prevented by marking the affected constructor with the keyword `explicit`:

```c++
class A {};

class B {
  public:
    explicit B (const A& x) {} // this constructor is marked as "explicit"
    B& operator= (const A& x) { return *this; }
    operator A() { return A(); }
};

void fn (B x) {}

int main ()
{
  A foo;
  B bar (foo);
  bar = foo;
  foo = bar;
  
  fn (foo); // Invalid! Not allowed for explicit constructor
  fn (bar);
}
```

Additionally, constructors marked with `explicit` cannot be called with the assignment-like syntax; In the above example, `bar` could not have been constructed with `B bar = foo;`. 

Type-cast member functions can also be specified as `explicit`, this prevents implicit conversions in the same way as Explicit-Specified Constructors do for the destination type.


## Type Casting

C++ is a strong-typed language. Many conversions, specially those that imply a different interpretation of the value, require an explicit conversion, known in C++ as “Type Casting". There are 2 main syntaxes for generic type-casting: "functional" and "c-like":
```c++
double x = 10.3;
int y;
y = int (x);    // functional notation
y = (int) x;    // c-like notation 
```

These syntaxes are enough for most situations with fundamental data types. While these operators, when applied on classes / pointers to classes, might lead to code that compiles without error but crash at runtime, like the code below:

```c++
// class type casting
#include <iostream>
using namespace std;

class Dummy {
    double i,j;
};

class Addition {
    int x, y;
  public:
    Addition (int a, int b) { x = a; y = b; }
    int result() { return x + y;}
};

int main () {
  Dummy d;
  Addition * pAdd;
  pAdd = (Addition*) &d; // 注意这一行！
  cout << pAdd->result() << endl;
}
```
The program declared a Pointer `pAdd` of type `Addition` (`Addition * pAdd;`), but then it was assigned a reference to an object `d` of an UNRELATED type `Dummy`, using an explicit type casting:
```c++
pAdd = (Addition*) &d;
```
Unrestricted Explicit Type Casting allows to convert ANY pointer into ANY other type of pointer. The subsequent code will produce either a run-time error or some other unexpected results.

In order to control the types of conversions between classes, we have 4 specific casting operators: `dynamic_cast`, `reinterpret_cast`, `static_cast` and `const_cast`:
```c++
dynamic_cast <new_type> (expression to be converted)
reinterpret_cast <new_type> (expression to be converted)
static_cast <new_type> (expression to be converted)
const_cast <new_type> (expression to be converted)
```
The traditional type casting equivalents to these would be:
```c++
(new_type) expression to be converted
new_type (expression to be converted)
```

Each of these 4 casting operators have their own special characteristics, 下面会分别讲，不要 fool around 啦。


## "dynamic_cast"

`dynamic_cast` can only be used with pointers and references to classes (or with `void*`). Its purpose is to ensure that the result of the type conversion points to a valid complete object of the destination pointer type.

This naturally includes **"Pointer Upcast"** (converting from a Pointer to a Derived Class to a Pointer to a Base Class), in the same way as allowed as an Implicit Conversion.

But `dynamic_cast` can also do the **"Pointer Downcast"** (convert from a Pointer to a Base Class to a Pinter to a Derived Class). It can "Downcast" a Polymorphic Classes (those with Virtual Members) **if and only if the pointed object is a valid complete object of the target derived type**.

```c++
#include <iostream>
#include <exception>
using namespace std;

class Base 
{
    virtual void dummy() {} // 这是一个虚函数，但不是一个"纯"虚函数
};

class Derived: public Base 
{
    int a;
};

int main () {
  try {
    Base * pB1 = new Derived;
    Base * pB2 = new Base;
    Derived * pD;

    pD = dynamic_cast<Derived*> (pB1);
    if (pD == 0) // 指针 == 0 意味着这是一个 Null Pointer!
      cout << "Null pointer on 1st type-cast.\n";

    pD = dynamic_cast<Derived*> (pB2);
    if (pD == 0) 
      cout << "Null pointer on 2nd type-cast.\n";

  } catch (exception& e) {
    cout << "Exception: " << e.what();
  }
}
// 最后的输出是：Null pointer on 2nd type-cast.

/* Compatibility Note: 
This type of `dynamic_cast` requires "Run-Time Type Information" (RTTI) to keep track of dynamic types. Some compilers support this feature as an option which is disabled by default. This needs to be enabled for runtime type checking using `dynamic_cast` to work properly with these types.
*/
```
The code above tries to perform 2 dynamic casts from pointer objects of type `Base*` (pB1 and pB2) to a pointer object of type `Derived*`, but only the 1st one is successful. Notice their respective initializations:
```c++
Base * pB1 = new Derived;
Base * pB2 = new Base;
```
Although both of them are pointers of type `Base*`, `pB1` actually points to an object of type `Derived`, while `pB2` points to an object of type `Base`. Therefore, when their respective type-casts are performed using `dynamic_cast`, `pB1` is pointing to a full object of class `Derived`, whereas `pB2` is pointing to an object of class `Base`, which is an **incomplete** object of class `Derived`.

When `dynamic_cast` cannot cast a pointer because it is not a complete object of the required class, it returns a `Null Pointer` to indicate the failure. If `dynamic_cast` is used to convert to a reference type and the conversion is not possible, an `exception` of type `bad_cast` is thrown instead.

`dynamic_cast` can also perform the other implicit casts allowed on pointers: casting `Null Pointers` between pointers types (even between unrelated classes), and casting any pointer of any type to a `void*` pointer.


## "static_cast"

`static_cast` can perform conversions between pointers to **related classes**, **not only "Upcasts"**, **but also "Downcasts"**. **NO checks are performed during runtime** to guarantee that the object being converted is in fact a full object of the destination type. Therefore, it is up to the programmer to ensure that the conversion is safe. On the other side, it does not incur the overhead of the type-safety checks of `dynamic_cast`.

```c++
class Base {};
class Derived: public Base {};

Base * a = new Base;
Derived * b = static_cast<Derived*> (a);
```

This would be valid code, although b would point to an incomplete object of the class and could lead to runtime errors if dereferenced. Therefore, `static_cast` is able to perform with pointers to classes not only the conversions allowed implicitly, but also their opposite conversions.

static_cast is also able to perform all conversions allowed implicitly (not only those with pointers to classes), and is also able to perform the opposite of these. It can:
* Convert from `void*` to any pointer type. In this case, it guarantees that if the `void*` value was obtained by converting from that same pointer type, the resulting pointer value is the same.
* Convert integers, floating-point values and `enum` types to `enum` types.

Additionally, `static_cast` can also perform the following:
* Explicitly call a single-argument constructor or a conversion operator.
* Convert to `rvalue references`.
* Convert `enum` class values into integers or floating-point values.
* Convert any type to `void`, evaluating and discarding the value.

## "reinterpret_cast"

`reinterpret_cast` converts any pointer type to any other pointer type, even of unrelated classes. The result is a simple binary copy of the value from one pointer to the other. All pointer conversions are allowed, neither the content pointed nor the pointer type itself is checked.

It can also cast pointers to or from "integer types". The format in which this integer value represents a pointer is platform-specific. The only guarantee is that a pointer cast to an integer type large enough to fully contain it (such as intptr_t), is guaranteed to be able to be cast back to a valid pointer.

The conversions that can be performed by `reinterpret_cast` but not by `static_cast` are low-level operations based on reinterpreting the binary representations of the types, which on most cases results in code which is system-specific, and thus non-portable. For example:

```c++
class A { ... };
class B { ... };

A * a = new A;
B * b = reinterpret_cast<B*> (a);
```

This code compiles, although it does not make much sense, since now b points to an object of a totally unrelated and likely incompatible class. Dereferencing b is unsafe.

## "const_cast"

This type of casting manipulates the "Constness" of the object pointed by a pointer, either to be set or to be removed. For example, in order to pass a const pointer to a function that expects a non-const argument:

```c++
#include <iostream>
using namespace std;

void print (char * str)
{
  cout << str << '\n';
}

int main ()
{
  const char * c = "sample text";
  print ( const_cast<char *> (c) ); // sample text
}
```

The example above is guaranteed to work because function `print` does not write to the pointed object. Note though, that removing the constness of a pointed object to actually write to it causes `undefined` behavior.

## typeid

`typeid` checks the type of an expression:
```c++
typeid (expression)
```

This operator returns a reference to a constant object of type `type_info` that is defined in the standard header `<typeinfo>`. A value returned by `typeid` can be compared with another value returned by `typeid` using operator `==` or `!=`, or can serve to obtain a null-terminated character sequence representing the data type or class name by using its `name()` member.

```c++
#include <iostream>
#include <typeinfo>
using namespace std;

int main () {
  int * a, b; // a是一个整形指针，b是一个整形变量
  a = 0;
  b = 0;

  if (typeid(a) != typeid(b))
  {
    cout << "a and b are of different types:\n";
    cout << "a is: " << typeid(a).name() << '\n'; // a is: int *
    cout << "b is: " << typeid(b).name() << '\n'; // b is: int
  }
  return 0;
}
```

When `typeid` is applied to classes, `typeid` uses the "RTTI" to keep track of the type of dynamic objects. When `typeid` is applied to an expression whose type is a Polymorphic class, the result is the type of **the most derived complete object**.

```c++
#include <iostream>
#include <typeinfo>
#include <exception>
using namespace std;

class Base
{ 
    virtual void f() {} 
};

class Derived : public Base {};

int main () {
  try {
    Base * a = new Base;
    Base * b = new Derived;

    cout << "a is: " << typeid(a).name() << '\n';   // a is: class Base *
    cout << "b is: " << typeid(b).name() << '\n';   // b is: class Base * 
    cout << "*a is: " << typeid(*a).name() << '\n'; // *a is: class Base
    cout << "*b is: " << typeid(*b).name() << '\n'; // *b is: class Derived
  } catch (exception& e) {
    cout << "Exception: " << e.what() << '\n';
  }
}
// Note: The string returned by typeid(x).name() depends on the specific implementation of your compiler and library. 
// It is not necessarily a simple string with its typical type name, like in the compiler used to produce this output. 
```

Notice how the type that `typeid` **considers for pointers is the pointer type itself** (both a and b are of type "class Base *"), but when `typeid` is applied **to objects** (like *a and *b) `typeid` **yields their "Dynamic Type" (i.e. the type of their most derived complete object)**.

If the type which `typeid` evaluates is a pointer preceded by the dereference operator `*`, and this pointer has a `null` value, typeid throws a `bad_typeid` exception.


## Reference

* [Type Conversions [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/typecasting/)

{% include links.html %}