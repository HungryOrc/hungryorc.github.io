---
title: "Class: Inheritance and Polymorphism"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_class_inheritance_polymorphism.html
folder: c++
toc: false
---

## Accessibility in Inheritance

Classes that are derived from others inherit all the **accessible** members of the base class. The members of the derived class can access the `protected` members inherited from the base class, but not its `private` members.

| Access | public | protected | private |
|:-------|:------:|:---------:|:-------:|
| members of the same class | Y | Y | Y
| members of derived classes of this class | Y | Y | N
| anywhere | Y | N | N

```c++
class Polygon // 多边形
{
    protected:
        int width, height;
    public:
        void setValues (int a, int b)
        {
            width = a;
            height = b;
        }
};

class Rectangle : public Polygon // 这个 public 限定了 class Rectangle 对父类的 最高继承权利 
{
    public:
        int area ()
        {
            return width * height;
        }
};

class Triangle : public Polygon
{
    public:
        int area ()
        {
            return width * height / 2;
        }
};

int main () 
{
    Rectangle rect;
    Triangle tria;
    rect.setValues (1, 2);
    tria.setValues (3, 4);
    cout << rect.area() << endl;
    cout << tria.area() << endl;
}
```

In the code above the `public` access specifier in the signatures of the derived classes (`Rectangle` and `Triangle`) may be replaced by `protected` or `private` in the derived classes. This access specifier limits the most accessible level for the members inherited from the base class: the members with a more accessible level are inherited with this level instead, while the members with an equal or more restrictive access level keep their restrictive level in the derived class. So the members inherited by `Rectangle` and `Triangle` have the same access permissions as they had in their base class `Polygon`:
```c+++
Polygon::width          // protected access
Polygon::setValues()    // public access

Rectangle::width        // protected access
Rectangle::setValues()  // public access 
```
This is because the inheritance relation has been declared using the `public` keyword in the signatures of the derived classes:
```c++ 
class Rectangle: public Polygon
class Triangle : public Polygon
```
This `public` keyword after the `:` denotes the most accessible level the members inherited from the class that follows it will have from the derived class. Since public is the most accessible level, by specifying this keyword the derived class will inherit all the members with the same levels they had in the base class. If the `public` keyword is replaced by `protected`, all `public` members of the base class are inherited as `protected` in the derived class. If replaced by `private`, all the base class members are inherited as `private`. But these access restrictions will not restrict the derived classes from declaring their OWN `public` or `protected` members, these restrictions are only set for the members inherited from the base class.

If no access level is specified for an inheritance relationship, the compiler assumes `private` for classes declared with keyword `class` and `public` for those declared with `struct`. Actually, most use cases of inheritance in C++ should use public inheritance. When other access levels are needed for base classes, they can usually be better represented as member variables instead.


## What is NOT inherited from the base class?

A publicly derived class inherits access to all the members of the base class EXCEPT the base class's:
* Constructors and Destructor
* Assignment Operator Members (operator=)
* Friends
* Private members

Although the Constructors and Destructor of the base class are not inherited, they are automatically called by the Constructors and Destructor of the derived class. Unless otherwise specified, the Constructors of a derived class calls the **Default** Constructor of its base class. Calling a different Constructor of the base class is doable by using the same syntax used to initialize member variables in the initialization list: `derivedConstructorName (parameters) : baseConstructorName (parameters) { ... }`. 执行的时候，是先执行父累的构造函数，再接着执行子类的构造函数:

```c++
#include <iostream>
using namespace std;

class Mother
{
    public: 
        Mother () { cout << "Mother Constructor: no parameter. "; }
        Mother (int a) { cout << "Mother Constructor: int parameter. "; }
};

class Daughter : public Mother
{
    public:
        Daughter (int a) // implicitly calling the default Constructor of the Mother class
        {
            cout << "Daughter Constructor: int parameter.\n";
        }
};

class Son : public Mother
{
    public:
        Son (int a) : Mother (a) // explicitly calling a non-default Constructor of the Mother class
        {
            cout << "Son Constructor: int parameter.\n";
        }
}

int main ()
{
    Daughter blueBaby(1); // Mother Constructor: no parameter. Daughter Constructor: int parameter.
    Son PurpleBaby(2); // Mother Constructor: int parameter. Son Constructor: int parameter.
}
```

## Multiple Inheritance

A class may inherit from more than one class by simply specifying more base classes, separated by commas:

```c++
#include <iostream>
using namespace std;

class Polygon {
  protected:
    int width, height;
  public:
    Polygon (int a, int b) : width(a), height(b) {}
};

class Output {
  public:
    static void print (int i) { cout << i << '\n'; }
};

class Triangle: public Polygon, public Output // Multiple Inheritance
{
  public:
    Triangle (int a, int b) : Polygon(a, b) {}
    int area () { return width * height / 2; }
};
  
int main () {
  Triangle tria (4, 5);
  tria.print (tria.area()); // 也可以这么写：Triangle::print (tria.area());
}
```

## Polymorphism

### Pointers to Base Class

One of the key features of class inheritance is that a pointer to a derived class is type-compatible with a pointer to its base class. **"Polymorphism"** is the art of taking advantage of this simple but powerful feature. The example above about the rectangle and triangle classes can be rewritten using pointers:

```c++
class Polygon {
  protected:
    int width, height;
  public:
    void setValues (int a, int b) { width = a; height = b; }
};

class Rectangle: public Polygon {
  public:
    int area() { return width * height; }
};

class Triangle: public Polygon {
  public:
    int area() { return width * height / 2; }
};

int main () {
  Rectangle rect;
  Triangle tria;
  Polygon * pPoly1 = &rect;
  Polygon * pPoly2 = &tria;

  pPoly1->setValues (4, 5);
  pPoly2->setValues (2, 10);
  cout << rect.area() << '\n';
  cout << tria.area() << '\n';
}
```

Function `main` declares 2 pointers to 2 `Polygon` type objects, these pointers are assigned the addresses of object `rect` and `tria`. Such assignments are valid since `rect` and `tira` are compatible to `Polygon` class.

`pPoly1->setValues(4, 5)` is the same to `rect.setValues(4, 5)`. But because the type of `pPoly1` and `pPoly2` is pointer to the class `Polygon`, not pointer to class `Rectangle` or `Triangle`, so only the members of `Polygon` can be accessed, not the members of `Rectangle` and `Triangle`. 

### Virtual Member Function

A **"Virtual Member"** or **"Virtual Member Function"** is a member function that can be redefined in a derived class, while preserving its calling properties through reference. The syntax for a function to become a virtual function is to precede its declaration with `virtual`. A class that declares or inherits a vertual function is a **"Polymorphic Class"**.

```c++
#include <iostream>
using namespace std;

class Polygon {
  protected:
    int width, height;
  public:
    void setValues (int a, int b) { width = a; height = b; }
    virtual int area () { return 0; } // Virtual Member Function
    // 注意！这虽然是个“虚函数”，但它的函数体并不虚！是一个合法的，完整的，可执行的函数体！
};

class Rectangle: public Polygon {
  public:
    int area () { return width * height; }
};

class Triangle: public Polygon {
  public:
    int area () { return (width * height / 2); }
};

int main () {
  Polygon poly;
  Rectangle rect;
  Triangle tria;
  Polygon * pPoly1 = &poly;
  Polygon * pPoly2 = &rect;
  Polygon * pPoly3 = &tria;
  
  pPoly1->setValues (4, 5);
  pPoly2->setValues (4, 5);
  pPoly3->setValues (4, 5);
  cout << pPoly1->area() << '\n'; // 0
  cout << pPoly2->area() << '\n'; // 20
  cout << pPoly3->area() << '\n'; // 10
}
```

`Polygon`, `Rectangle` and `Triangle` have the same members: `width`, `height`, functions `setValues` and `area`. The member function `area` has been declared as `virtual` in the base class. Actually, **non-virtual members can also be redefined in the derived classes**, but non-virtual members of the derived **classes cannot be accessed by a pointer of the base class**. So if the keyword `virtual` is removed from the declaration of `area` in `Polygon`, all 3 calls to `area` would return 0, because in all cases, the version of `area` in the base class would have been called instead.

Essentially what the `virtual` keyword does is to allow a member of a derived class with the same name as one in the base class, to be appropriately called from a pointer which has the type of the base class and is pointing to an object of a derived class, as `pPoly2` and `pPoly3` in the example above.

Note that despite of the virtuality of one of its members, `Polygon` was a regular class, which can be instantiated.

### Abstract (Base) Classes

**"Abstract (Base) Classes"** are very similar to the class `Polygon` in the previous section, they are classes that **can ONLY be used as base classes**, and thus are allowed to **have Virtual Member Functions without definition** (known as "Pure Virtual Functions"). Classes that contain at least one "Pure Virtual Function" are known as Abstract Base Classes. The syntax for defining a Pure Virtual Function is to replace the function definition `{ ... }` by `= 0`. 注意区别以下两种情况：
```c++
virtual int function1 () =0;  // "Pure" Virtual Function，注意 "=0" 这个特殊的标记
virtual int function2 () {}   // 这不是一个 "Pure" 虚函数！只是一个普通的 虚函数！一个body为空即什么也不干的虚函数
```

```c++
// Abstract Base Class
class Polygon {
  protected:
    int width, height;
  public:
    void setValues (int a, int b) { width=a; height=b; }
    virtual int area () =0; // Pure Virtual Function
};
```

Abstract Base Classes can have non-virtual member functions, as `setValues` in the example above. But still, an Abstract Base Class cannot be used to instantiate objects. **It can be used to create Pointers to itself, and take advantage of all its Polymorphic abilities**. For example:
```c++
Polygon * pPoly1; // valid
Polygon myPolygon1; // invalid, since Polygon is an Abstract Base Class
```

**These Pointers of an Abstract Base Classes type can be dereferenced when they are actually pointing to objects of derived non-abstract classes**, like the following:
```c++
#include <iostream>
using namespace std;

// 因为含有一个 虚函数，所以 Polygon 这个类成了 抽象类，所以它不能再被直接 实例化，
// 但是继承自它的类，完全可以是 非抽象类，只要这些类不含有任何 虚函数，那么这些类都是可以被 实例化的，
// 那么我们完全可以搞一个指针，声明这个指针的时候设定它的类型为Polygon，然后赋值的时候赋值为一个非抽象的子类的对象，就ok了
class Polygon {
  protected:
    int width, height;
  public:
    void setValues (int a, int b) { width = a; height = b; }
    virtual int area () =0;
};

class Rectangle: public Polygon {
  public:
    int area () { return (width * height); }
};

class Triangle: public Polygon {
  public:
    int area () { return (width * height / 2); }
};

int main () {
  Rectangle rect;
  Triangle tria;
  Polygon * ppoly1 = &rect;
  Polygon * ppoly2 = &tria;

  ppoly1->setValues (4, 5);
  ppoly2->setValues (4, 5);
  cout << ppoly1->area() << '\n'; // 20
  cout << ppoly2->area() << '\n'; // 10
}
```

In this example, objects of **different but related types** are referred to **using the same type of Pointer** `Polygon *` and the proper member function is called every time, just because they are virtual. It is even possible for a member of the Abstract Base Class `Polygon` to use the special pointer `this` and the arrow operator `->` to **access the proper virtual members**, even though `Polygon` itself has no implementation for this function:
```c++
class Polygon {
  protected:
    int width, height;
  public:
    void setValues (int a, int b) { width = a; height = b; }
    virtual int area() =0;
    void printArea() { cout << this->area() << '\n'; } // use "this->" to access virtual member!
};

class Rectangle: public Polygon {
  public:
    int area (void) { return (width * height); }
};

int main () {
  Rectangle rect;
  Polygon * pPoly1 = &rect;

  pPoly1->setValues (4, 5);
  pPoly1->printArea(); // 20
}
```

Virtual Members and Abstract Classes grant C++ Polymorphic characteristics.

## 综合例子

An example combining Dynamic Memory Allocation, Constructor Initializer, Polymorphism and so on:

```c++
#include <iostream>
using namespace std;

class Polygon
{
    protected:
        int width, height;
    public:
        Polygon (int a, int b) : width(a), height(b) {}
        virtual int area () =0;
        void printArea() { cout << this->area() << endl; }
};

class Rectangle : public Polygon
{
    public:
        Rectangle (int a, int b) : Polygon (a, b) {}
        int area() { return width * height; }
};

int main ()
{
    // This Pointer is declared being of type "pointer to Polygon",
    // but the objects allocated have been declared having the derived class type "Rectangle" directly
    Polygon * pPoly1 = new Rectangle (4, 5); // 动态分配内存
    pPoly1->printArea();
    
    delete pPoly1;
}
```




## Reference

* [Inheritance [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/inheritance/)

{% include links.html %}