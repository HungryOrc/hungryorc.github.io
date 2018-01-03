---
title: "Function Template in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_function_template.html
folder: c++
toc: false
---

```c++
// example of template
#include <iostream>
using namespace std;

template <class T>
T sum (T a, T b)
{
  T result = a + b;
  return result;
}

int main ()
{
  cout << sum<int>(1, 2) << '\n';
  cout << sum<double>(-0.1, 0.5) << '\n';
}
```

```c++
// another example of template:
// the parameters belong to different kinds of generic types, 
// and the function returns a basic type (namely non-generic type)
#include <iostream>
using namespace std;

template <class T, class U>
bool isEqual (T a, U b)
{
  return (a == b);
}

int main()
{
  if (isEqual<int, double>(10, 10.0)) // 也可以写为 isEqual(10, 10.0)
  {
      cout << "Equal!\n";
  }
  else
  {
      cout << "Not equal!\n";
  }
}
```

## Template Arguments that are Non-type

The template parameters can not only include types introduced by class or typename, but can also include expressions of a particular type:

```c++
// template arguments
#include <iostream>
using namespace std;

template <class T, int N> // the 2nd argument N is of a basic type, int
T fixed_multiply (T val)
{
  return val * N;
}

int main() {
  cout << fixed_multiply<int,2>(10) << '\n';
  cout << fixed_multiply<int,3>(10) << '\n';
}
```

The second argument (`int N`) of the fixed_multiply function template is of type int. It just looks like a regular function parameter, and can actually be used just like one.

But there exists a major difference: the value of template parameters is determined on compile-time to generate a different instantiation of the function fixed_multiply, and thus the value of that argument is never passed during runtime: The two calls to fixed_multiply in main essentially call two versions of the function: one that always multiplies by 2, and one that always multiplies by 3. For that same reason, the second template argument needs to be a constant expression (it cannot be passed a variable).

## Reference

* [Overloads and Templates](http://www.cplusplus.com/doc/tutorial/functions2/)

{% include links.html %}