---
title: "Class in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_class.html
folder: c++
toc: false
---

## Declaration

```c++
class class_name {
    access_specifier_1: member1;
    access_specifier_2: member2;
    ...
} object_names;
```
`class_name` is a valid identifier for the class. `object_names` is an optional list of names for objects of this class. `access specifiers` are optional.

### Access Specifier

"Access Specifier" is one of the following 3 keywords: `private`, `public` or `protected`:

* `private` members of a class are accessible only from within **other members of the same class** (i.e. `friends`).
* `protected` members are accessible from **other members of the same class** (i.e. `friends`), but also from **members of their derived classes**.
* `public` members are accessible from **anywhere where the object is visible**.
* By default, all members of a class declared with the `class` keyword have `private` access.

```c++
class Rectangle {
    int width, height;
  public:
    void set_values (int, int); // 函数只是声明，没有定义具体内容的时候，形参只规定类型，不规定形参名！
    int area ()
    {
        return width * height;
    }
} rect; // "Rectangle" was the class name, "rect" was an object of type Rectangle

// 注意下面的语法！在 class定义 的外面，补充定义class的成员函数，需要用 ::
// :: 叫做 "Scope Operator"。在 namespace 那边也有运用。
void Rectangle::set_values (int x, int y) // 函数定义的时候，形参就需要形参名了
{
  width = x; // private 的 width 和 height，可以这样放在这里直接用！不需要 this. 之类的前缀
  height = y;
}

```

The above code declares a class called `Rectangle` and an object of this class, called `rect`. This class contains 4 members: 2 data members of type int (`width`, `height`) with private access (because private is the default access level) and 2 member functions with public access (`set_values`, `area`), of which for now we have only included their declaration, but not their definition.

## Constructor

To initialize member variables properly before any function might use them, or allocate storage, or anything else that shall be done in advance, `constructor` is what you deserve hahaha! `constructor` function is declared just like a regular member function, but with a name that matches the class name, and without any return type, not even `void`. And natually, a `constructor` has to be `public`. Constructors cannot be called explicitly as if they were regular member functions. They are only executed once, when a new object of that class is created.

```c++
class Rectangle {
    int width, height;
  public:
    Rectangle (int, int); // declaration of constructor, no return type
    ...
};

Rectangle::Rectangle (int a, int b) { // definition of constructor, no return type
  width = a;
  height = b;
}

int main () {
  Rectangle rect1 (3, 4);
  Rectangle rect2 (5, 6);
  ...
}
```

## Overloading Constructors

Like any other function, a constructor can also be overloaded with different versions taking different parameters: with a different number of parameters and/or parameters of different types. The compiler will automatically call the one whose parameters match the arguments:

```c++
class Rectangle {
    int width, height;
  public:
    Rectangle (); // 构造函数一
    Rectangle (int, int); // 构造函数二
    Rectangle (int w) // 构造函数三，inlined
    {
        width = w;
        height = 2 * w;
    }
    ...
};

Rectangle::Rectangle () {
  width = 5;
  height = 5;
}

Rectangle::Rectangle (int a, int b) {
  width = a;
  height = b;
}

int main () {
  Rectangle rect1 (3, 4);
  Rectangle rect2; // 没有参数的构造函数，连空的括号 () 都不用了！
  Rectangle rect3 (2);
  ...
}
```
This example also introduces a special kind constructor: the `default constructor`, which is the constructor that takes no parameters. Note how `rect2` is constructed without even an empty set of parentheses, empty parentheses cannot be used to call the default constructor. If we do it like `Rectangle rect2()`, the constructor will not be called, since the `()` will make `rect2()` to be a function declaration instead of a call of the constructor function, it would be a function that takes no arguments and returns a value of type `Rectangle`.

## Member Initialization in Constructors

When a constructor is used to initialize members of the class, these other members can be initialized directly, without resorting to statements in the constructor's body. This is done by inserting a colon `:` and a list of initializations for class members before the constructor's body. For example the following constructor:
```c++
Rectangle::Rectangle (int x, int y) { width = x; height = y; }
```
Can also be defined using "Member Initialization" as:
```c++
Rectangle::Rectangle (int x, int y) : width(x), height(y) {}
// or:
Rectangle::Rectangle (int x, int y) : width(x) { height = y; }
```

For members of fundamental types, it makes no difference which of the ways above the constructor is defined, because they are not "initialized by default", but for "member objects" whose type is a class, **if they are not initialized after the `:`, they are default-constructed**. While default-constructing all members of a class may or may always not be convenient: in some cases, this is a waste (when the member is then reinitialized otherwise in the constructor), in some other cases, this is not even possible (when the class does not have a default constructor). In these cases, members shall be initialized in the member initialization list. For example:
```c++
class Circle {
    double radius;
  public:
    Circle(double r) : radius(r) {}
    double area() { return radius * radius * 3.14159265; }
};

class Cylinder {
    Circle base;
    double height;
  public:
    Cylinder(double r, double h) : base (r), height(h) {}
    double volume() { return base.area() * height; }
};
```
In the example above, class `Cylinder` has a member `base` whose type is `Circle`. Because objects of class `Circle` can only be constructed with one parameter, `Cylinder`'s constructor needs to call `base`'s constructor, and **the ONLY way to do this is in the member initializer list**, 否则，如前文所述，如果不在 member initialization list 里面初始化 class type member，那么这个 member 将会被 default-constructed，而那样很可能不是我们想要的，甚至可能引发error。

## Pointers to Classes, Arrow Operator, Dynamic Memory Allocation with "new"

Once declared, a class becomes a valid type, so `Rectangle * rectPointer` is a Pointer to an object of class `Rectangle`.

Similarly, as with `struct`s, the members of an object can be directly accessed from a pointer by using the Arrow Operator `->`:
```c++
Rectangle rect1 (3, 4);
Rectangle * rectP1 = &rect1;

// 注意 new 关键字的运用！这是内存的动态分配！
Rectangle * rectP2 = new Rectangle (5, 7);
Rectangle * rectP3 = new Rectangle[2] { {5, 10}, { 3, 1} }; // an array of Rectangle objects
```

一个小结：
* `x.y`: member `y` of object `x`
* `x->y`: member `y` of the object which is pointed to by Pointer `x`
* `(*x).y`: equivalent to `x->y`
* `x[0]`: 1st object pointed to by Pointer `x`
* `x[n]`: (n+1)th object pointed to by Pointer `x`

## Keyword "this"

The keyword `this` represents a **Pointer** to the object whose member function is being executed. It is used within a class's member function to refer to the object itself.

```c++
class Dummy {
  public:
    bool isitme (Dummy& param);
};

bool Dummy::isitme (Dummy& param)
{
  if (&param == this) return true;
  else return false;
}

int main () {
  Dummy a;
  Dummy* b = &a;
  if (b->isitme(a))
  {
    cout << "yes, &a is b\n";
  }
}
```

## Static Members, Data or Function

### Static Data

```c++
class Dummy {
  public:
    static int n;
    Dummy () { n++; };
};

int Dummy::n = 0;

int main () {
  Dummy a;
  Dummy b[5]; // 建立了一个Dummy类型的数组，里面有5个Dummy类型的objects
  cout << a.n << '\n'; // 会显示 6
  Dummy * c = new Dummy;
  cout << Dummy::n << '\n'; // 会显示 7
  delete c;
  cout << a.n << '\n'; // 虽然删除了 c，但这里还是会显示 7，因为一共曾经创造过7个，而delete的定义并没有 n-- 的内容
}
```

读写静态变量有2种方式，在上面的例子里都有体现：
* 方式1，用 “class名::静态变量名” 这样的格式，如 `Dummy::n`
* 方式2，用 “此class的object名.静态变量名” 这样的格式，如 `a.n`

Static members have the same properties as non-member variables but they enjoy class scope. For that reason, and **to avoid them to be declared several times**, **they cannot be initialized directly in the class**, but need to be initialized somewhere outside.

### Static Function

Static Member Functions are members of a class that are common to all object of that class. Since they are like non-member functions, **they cannot access non-static members of the class** (neither non-static variables nor non-static functions). **They can neither use the keyword `this`**. ———— 为什么要规定它们不可以用 this ？？？？？？？


## Const Member Functions

### Const Object

定义了一个 class 以后，我们可以定义属于此类的某个object为 "`const` object"，语法为: `const ClassName objectName`。一个 `const` object 的所有 data members 对于外部都是 `const` 的 (或者说 Read-only 的)。不过初始化这个object的constructor自然是可以 initialize 以及 modify 它的 data members 的。
```c++
class MyClass {
  public:
    int x;
    MyClass(int val) : x(val) {}
    int get() { return x; }
};

int main() {
  const MyClass foo(10); // 定义了一个 const object
  foo.x = 20; // invalid, x cannot be modified
}
```

### Const Member Function

`const` object 的 member function can only be called if these functions are themselves specified as `const` members. in the example above, member function `get` cannot be called from the class in which it resides. 声明 `const` member function 的方法：

```c++
int get() const { ... }
```

`const` can also be used to qualify the type returned by a member function. This `const` is not the same as the one which specifies a member as `const`, they just use the same key word, that's it. Both are independent and are located at different places in the function prototype:

```c++
int get() const { ... } // const member function
const int& get() { ... } // non-const member function returning a const int&
const int& get() const { ... } // const member function returning a const int& 
```

Member functions specified to be `const` cannot modify "non-static data members" nor call other "non-const member functions". In essence, **const members shall not modify the state of an object**.

`const` objects are limited to access only member functions marked as `const`. Non-const objects can access both const and non-const member functions.

You may think that anyway you are seldom going to declare const objects, and thus marking all members that don't modify the object as const is not worth the effort. But const objects are actually very common. Many functions **taking classes as parameters take them by const reference**, so these functions can only access the const members of the classes. In the following example, `get` was specified as a `const` member, so the call to `arg.get()` in the `print` function was possible, because `const` objects only have access to `const` members (functions or data).
```c++
class MyClass {
    int x;
  public:
    MyClass(int val) : x(val) {}
    const int& get() const { return x; }
};

void print (const MyClass& arg) { // MyClass& 就是一个 const reference
  cout << arg.get() << '\n';
}

int main() {
  MyClass foo (10);
  print(foo);
}
```

### Overloaded Const Member Function

A class may have 2 member functions with identical signatures except that one is const and the other is not. The const version is called only when the object is itself const, and the non-const version is called when the object is itself non-const. Oh yeah!
```c++
class MyClass {
    int x;
  public:
    MyClass(int val) : x(val) {}
    const int& get() const {return x;}
    int& get() {return x;}
};

int main() {
  MyClass foo (10);
  const MyClass bar (20);
  foo.get() = 15; // valid
  bar.get() = 25; // invalid! `bar.get()` returns `const int&`, which cannot be modified
}
```


## Class Templates

Like "Function Templates", "Class Templates" allow classes to have members that use the parameters of the template as their types:
```c++
template <class T> // this T is the template parameter
class myPair {
    T a, b;
  public:
    myPair (T first, T second) // constructor
    {
      a = first;
      b = second;
    }
    T getMax ();
};

template <class T> // this T is the template parameter
// the following line has 2 T:
// the 1st T is the return type of the function, 
// the 2nd T specified that this function's template parameter is also the class templete parameter
T myPair<T>::getMax () 
{
  T result;
  result = a>b ? a : b;
  return result;
}

int main () {
  // "myPair" is the class name
  // "<int>" is the parameter-type that substitute the generic type "T"
  // "myInteger" is the object name
  myPair<int> myIntegers (100, 75);
  cout << myobject.getMax();
}
```

## Template Specialization

We can define a different implementation for a template when a specific type is passed as the template parameter. This is called a "Template Specialization (特化)". For example, let's suppose that we have a very simple class called mycontainer that can store one element of any type and that has just one member function `increase`, which increases its value. But we find that when it stores an element of type char it would be more convenient to have a completely different implementation with a function member `uppercase`, so we decide to declare a class template specialization for that type:

```c++
// class template
template <class T>
class myContainer {
    T element;
  public:
    myContainer (T arg) { element = arg; }
    T increase () { return ++element; }
};

// class template specializations
// all types are known and no template arguments are required for this specialization
// but still, it is the specialization of a class template, so it requires to be noted as "template <>"
template <> // 这里不要再写 T 了
class myContainer <char> { // 仅针对 char 这个类型
    char element;
  public:
    myContainer (char arg) { element = arg; }
    char uppercase ()
    {
      if ((element >= 'a') && (element <= 'z'))
        element += 'A' - 'a';
      return element;
    }
};

int main () {
  myContainer<int> myint (7);
  myContainer<char> mychar ('j');
  cout << myint.increase() << endl;
  cout << mychar.uppercase() << endl;
}
```

Notice the differences between the generic class template and the specialization:
```c++
template <class T> class mycontainer { ... };
template <> class mycontainer <char> { ... };
```

When we declare specializations for a template class, we must also define ALL its members, even those identical to the generic template class, because there is no "inheritance" of members from the generic template to the specialization.


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

When an object (A) is passed an object (B) of its own type as argument, its **Copy Constructor** is invoked in order to construct a copy of B. 

"Copy Constructor" is a constructor whose 1st parameter is of **type reference** to the class itself (possibly `const` qualified) and which can be invoked with a single argument of this type. For example, for a class `MyClass`, the explicit Copy Constructor might have the following signature:
```c++
// Explicit Copy Constructor
MyClass::MyClass (const MyClass&); 
```

If a class has no custom "Copy Constructor/Assignment" nor "Move Constructor/Assignment" defined, an "IMPLICIT Copy Constructor" is automatically defined. For example, for a class such as:
```c++
class MyClass {
  public:
    int a, b; string c;
};
```
The Implicit Copy Constructor defined for this class will perform a **Shallow Copy** of the members of the class, roughly equivalent to:
```c++
// underlying form of the Implicit Copy Constructor
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
    Example5 (const string& str) : ptr(new string(str)) {}
    ~Example5 () {delete ptr;}
    // Copy Constructor: performs Deep Copy
    Example5 (const Example5& x) : ptr(new string(x.content())) {}
    
    const string& content() const {return *ptr;}
};

int main () {
  Example5 foo ("Example");
  Example5 bar = foo; // object initialization: Copy Constructor called, 相当于 Example5 bar (foo)
  ...
}
```
The Deep Copy performed by this Copy Constructor allocates storage for a new string, which is initialized to contain a copy of the original object. In this way, both objects (copy and original) have distinct copies of the content stored in different locations.

### Copy Assignment

```c++
MyClass foo;
MyClass bar (foo); // object initialization for baz: Copy Constructor called
MyClass baz = foo; // object initialization for baz: Copy Constructor called
```
```c++
MyClass foo;
foo = bar; // object foo had already been initialized: so this is a copy assignment, not a Copy Constructor
```
In `MyClass baz = foo`, `baz` is **initialized on construction using an equal sign, this is not an assignment operation** (although it may look like one)! It is **one way of the syntaxes to call the single-argument constructors**.









## Reference

* [Classes (I) [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/classes/)
* [Classes (II) [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/templates/)
* [Special Members of Classes [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/classes2/)

{% include links.html %}