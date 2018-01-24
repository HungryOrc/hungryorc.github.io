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
`MyClass baz = foo` is to **initialize** `baz` **on construction USING AN EQUAL SIGN**. This is **just another syntax to CALL A SINGLE-ARGUMENT CONSTRUCTOR**. This is NOT an Assignment Operation, although it looks like one. This is NEITHER a Copy Assignment.        

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

But again, the **Implicit** version of Copy Assignment Operator performs a **Shall Copy**. So in this case, for classes with Pointers as their member data, not only the classes incur the risk of deleting the same pointed object twice, but also **the Assignment creates Memory Leaks** by not deleting the object pointed by the object before the assignment. These issues can be solved by customizing a Copy Assignment that **deleted the previous object** and performs a **Deep Copy**:

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

Similar to "copying" an object, "moving" an object also uses the value of an object (source) to set the value to another object (destination). But unlike copying, on moving the "source object" will LOSE that content, which is TAKEN OVER by the "destination object". This moving only happens when the source of the value is an "unnamed Object".

"Unnamed Objects" are object that are temporary in natuare, and haven't even been given a name. Typical examples of Unnamed Objects are: return values of functions, type-casts... Using the value of a temporary object such as these to initialize another object or to assign its value does not require a copy, the source object is never going to be used anywhere else. So its value can be **moved into** the destination object. 所以我们会用到 **"Move Constructor"** and **"Move Assignment"**:

* "Move Constructor" is called when an object is initialized on construction using an unnamed temporary object
* "Move Assignment" is called when an object is assigned the value of an unnamed temporary object.
```c++
// MyClass fn(); // function returning a MyClass object

MyClass foo;         // default Constructor (which takes no argument)
MyClass bar = foo;   // Copy Constructor
foo = bar;           // Copy Assignment
MyClass baz = fn();  // Move Constructor
baz = MyClass();     // Move Assignment
```
In the code above, both the value returned by `fn()` and the value constructed with `MyClass()` are unnamed temporaries. In these cases, there is no need to make a copy. 

The Move Constructor and Move Assignment are members that takes a parameter of type `rvalue reference` to the class itself:
```c++
MyClass (MyClass&&);            // Move Constructor
MyClass operator= (MyClass&&);  // Move Assignment
```
A `rvalue reference` is specified by following the type with `&&`. As a parameter, an `rvalue reference` matches arguments of temporaries of this type. 

The concept of "moving" is most useful for objects that manage the storage they use, such as objects that allocate storage with `new` and `delete`. In such objects, "copying" and "moving " are quite different operations:
* Copying from A to B means: new memory is allocated to B, and then the entire content of A is copied to this new memory block, and A is not changed
* Moving from A to B means: the memory that had been allocated to A is "TRANSFERRED" to B without allocating any new storage. It involves simply copying the Pointer. A occupies no memory after this.

```c++
#include <iostream>
#include <string>
using namespace std;

class Example6 {
    string* ptr;
  public:
    Example6 (const string& str) : ptr(new string(str)) {}
    ~Example6 () {delete ptr;}
    // Move Constructor
    Example6 (Example6&& x) : ptr(x.ptr) {x.ptr = nullptr;}
    // Move Assignment
    Example6& operator= (Example6&& x) {
      delete ptr; 
      ptr = x.ptr;
      x.ptr = nullptr;
      return *this;
    }
    
    const string& content() const {return *ptr;}
    
    Example6 operator+ (const Example6& rhs) {
      return Example6(content() + rhs.content());
    }
};


int main () {
  Example6 foo ("Exam");
  Example6 bar = Example6("ple");   // Move Construction
  foo = foo + bar;                  // Move Assignment
  cout << "foo's content: " << foo.content() << '\n'; // "Example"
}
```

Although `rvalue references` can be used for the type of any function parameter, it is seldom useful for uses other than the Move Constructors. `rvalue reference` is tricky, and unnecessary uses of it may become the source of errors that are quite difficult to debug. Oh yeah!


## 小结

| Member Function | Implicitly Defined When | This Implicit Version Will Do |
|:-----------|:------------|:------------|
| Default Constructor  | if no other Constructors are defined  | do nothing     
| Destructor  | if no Destructor is defined  | do nothing    
| Copy Constructor  | if no Move Constructor && no Move Assignment  | copy all the members
| Copy Assignment  | if no Move Constructor && no Move Assignment  | copy all the members
| Move Constructor  | if no Destructor && no Copy Constructor && no Copy Assignment && no Move Assignment  | copy all the members
| Move Assignment  | if no Destructor && no Copy Constructor && no Copy Assignment && no Move Assignment  | copy all the members

Notice how not all Implicit Member Functions are implicitly defined in the same cases. This is mostly due to backwards compatibility with C structures and earlier C++ versions, and in fact some include deprecated cases. Fortunately, each class can select explicitly which of these members exist with their default definition or which are deleted by using the keywords `default` and `delete` respectively. The syntax is either one of:
```c++
function_declaration = default;
function_declaration = delete;
```
```c++
class Rectangle {
    int width, height;
  public:
    Rectangle (int x, int y) : width(x), height(y) {}
    Rectangle() = default; // 设置 default
    Rectangle (const Rectangle& other) = delete; // 用 delete 断绝了这个 Copy Constructor
    int area() {return width * height;}
};

int main () {
  Rectangle foo;
  Rectangle bar (10, 20);
  cout << "bar's area: " << bar.area() << '\n'; // 200
}
```

`Rectangle` can be constructed either with 2 int arguments or be Default-Constructed with no arguments. It can NOT be Copy-Constructed from another Rectangle object, because this function has been "deleted". So a Copy Construction like `Rectangle baz (foo)` is invalid. 

It could be made valid by defining its Copy Constructor with `= default`:
```c++
Rectangle::Rectangle (const Rectangle& other) = default;
```
Which would be essentially equivalent to:
```c++
Rectangle::Rectangle (const Rectangle& other) : width(other.width), height(other.height) {}
```
Essentially keyword `default` does NOT define a member function to be the Default Constructor (Default Constructor means Constructor with no parameters), but to be **the Constructor that would be implicitly defined if not deleted**.


## Reference

* [Special Members of Classes [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/classes2/)

{% include links.html %}