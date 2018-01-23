---
title: "\"struct\" in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_struct.html
folder: c++
toc: false
---

## Declaration

```c++
struct product {
  int weight;
  double price;
};

product apple, banana;

// 或者如下这样也是同样的效果：
struct product {
  int weight;
  double price;
} apple, banana;

// 还可以声明一个用这种struct组成的数组：
struct product {
  int weight;
  double price;
} items[5];
```


## Example

```c++
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
} films [3];

void printMovie (movies_t movie);

int main ()
{
  string mystr;
  int n;

  for (n = 0; n < 3; n ++)
  {
    cout << "Enter title: ";
    getline (cin, films[n].title);
    cout << "Enter year: ";
    getline (cin, mystr);
    stringstream(mystr) >> films[n].year;
  }

  cout << "\nYou have entered these movies:\n";
  for (n = 0; n < 3; n ++)
    printMovie (films[n]);
}

void printMovie (movies_t movie)
{
  cout << movie.title;
  cout << " (" << movie.year << ")\n";
}
```

## Pointer to struct, and the "->" Operator

Like any other type, structures can be pointed to by its own type of pointers:

```c++
struct movies_t {
  string title;
  int year;
};

movies_t movie1;

movies_t * moviePointer; // movies_t 类型 的指针
moviePointer = &movie1;
```

```c++
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
};

int main ()
{
  string mystr;

  movies_t amovie;
  movies_t * pmovie;
  pmovie = &amovie;

  cout << "Enter title: ";
  getline (cin, pmovie->title);
  cout << "Enter year: ";
  getline (cin, mystr);
  (stringstream) mystr >> pmovie->year;

  cout << "\nYou have entered:\n";
  cout << pmovie->title;
  cout << " (" << pmovie->year << ")\n";
}
```
In the example above, the **Arrow Operator** `->` is a dereference operator that is used exclusively with pointers to objects that have members, it serves to **access the member of an object directly from its address**. `pmovie->title` is equivalent to `(*pmovie).title`. 注意区别：`*pmovie.title` and `*(pmovie.title)` (these two are equivalent) would access the value pointed to by a hypothetical pointer-type-member called `title` of the struct object `pmovie` (which in this case makes no sense, since `pmovie` is not a struct but a pointer, and it does not have a member called `title`, and the member `title` of the struct `movies_t` is not of pointer type, it's of string type).

## Nested Structures

```c++
struct movies_t {
    string title;
    int year;
};

struct friends_t {
    string name;
    string address;
    int cellphone;
    movies_t favoriteMovie;
} charlie, daniel;

friends_t * friendPointer = &daniel;

daniel.favorite_movie.title = "Cube";
friendPointer->favorite_movie.year = 2002;
```



## Reference

* [Data Structures](http://www.cplusplus.com/doc/tutorial/structures/)

{% include links.html %}