---
title: "Class: Friend"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_class_friend.html
folder: c++
toc: false
---

## Friend Functions

"Friends" are functions or classes declared with keyword `friend`. Usually private and protected members of a class cannot be accessed from outside the class, but a non-member function can access the private and protected members of a class if it is declared as **friend of that class**. That is done by including a declaration of this external function within the class, and preceding it with keyword `friend`:

```c++
class Rectangle {
    int width, height;
  public:
    Rectangle() {}
    Rectangle (int x, int y) : width(x), height(y) {}
    int area() {return width * height;}
    friend Rectangle duplicate (const Rectangle&); // 朋友圈
};

Rectangle duplicate (const Rectangle& param)
{
  Rectangle result;
  result.width = param.width * 2;
  result.height = param.height * 2;
  return result;
}

int main () {
  Rectangle foo;
  Rectangle bar (2,3);
  foo = duplicate (bar);
  cout << foo.area() << '\n';
}
```

The function `duplicate` is a `friend` of class `Rectangle`. Therefore, `duplicate` is able to access the members `width` and `height` (which are private) of different objects of type `Rectangle`. But still, the friend functions are not member functions of the class.

Typical use cases of friend functions are operations that are conducted between two different classes accessing private or protected members of both classes. 

## Friend Class

Similar to "Friend Functions", a "Friend Class" is **a class whose members have access to the private or protected members of another class**:

```c++
class Square;

class Rectangle {
    int width, height;
  public:
    int area () {return (width * height);}
    void convert (Square a);
};

class Square {
  friend class Rectangle; // 朋友圈
  private:
    int side;
  public:
    Square (int a) : side(a) {}
};

void Rectangle::convert (Square a) {
  width = a.side;
  height = a.side;
}
  
int main () {
  Rectangle rect;
  Square sqr (4);
  rect.convert(sqr);
  cout << rect.area();
}
```




## Reference

* [Friendship [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/inheritance/)

{% include links.html %}