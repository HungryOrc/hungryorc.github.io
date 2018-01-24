---
title: "Enumerated Types: \"enum\" in C++"
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_enum.html
folder: c++
toc: false
---

## Declaration

Enumerated Types are types that are defined with a set of "custom identifiers", known as "enumerators", as possible values. Objects of these enumerated types can take any of these enumerators as value.

```c++
enum type_name
{
  value1,
  value2,
  ...
  ...
} object_names;
```
This creates the type `type_name`, which can take any of `value1`, `value2`... as its value. Objects (variables) of this type can directly be instantiated as `object_names`.

```c++
enum colors_t
{
    black, blue, green, cyan, red, purple, yellow, white
};

colors_t mycolor = blue;
if (mycolor == green) 
{
    mycolor = red; 
}
```

Values of enumerated types declared with `enum` are implicitly convertible to an integer type, and also backwards. The elements of such an `enum` are always assigned an integer numerical equivalent internally, to which they can be implicitly converted to or from. If it is not specified otherwise, the integer value equivalent to the 1st possible value is 0, to the 2nd is 1, to the 3rd is 2, and so on.

A specific integer value can be specified for any of the possible values in the enumerated type. If the value that follows it is itself not given its own value, it is automatically assumed to be the same value + 1. For example:
```c++
enum months_t
{
    january = 5, february, march, april,
    may, june, july, august,
    september, october, november, december
} y2k;
```
In this case, the variable `y2k` of the enumerated type `months_t` can be anything between "january" to "december" and that are equivalent to the values between 5 and 16 (not between 0 and 11, since january has been made equal to 1). The implicit conversion works **both ways**: a value of type months_t can be assigned a value of 5 (which would be equivalent to january), or an integer variable can be assigned a value of january (equivalent to 5).

## Enumerated Types with enum Class

In C++, it's possible to create **real enum types** that are neither implicitly convertible to int, nor have enumerator values of type int, but of the "enum type" itself, thus preserving **type safety**. They are declared with `enum class` or `enum struct` instead of just `enum`:

```c++
enum class Colors 
{
    black, blue, green, cyan, red, purple, yellow, white
};
```
Each of the enumerator values of an `enum class` type needs to be **scoped into its type** (this is actually also possible with `enum` types, but it is only optional). For example:

```c++
Colors mycolor;
 
mycolor = Colors::blue;
if (mycolor == Colors::green)
{
    mycolor = Colors::red;
}
```

Enumerated types declared with `enum class` also have more control over their **underlying type**; it may be any integral data type, such as char, short or unsigned int, which essentially serves to determine the size of the type, for example:

```c++
// "Eyecolor" is a distinct type with the same size of a char (1 byte)
enum class EyeColor : char 
{
    blue, green, brown
}; 
```



## Reference

* [Enumerated Types [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/other_data_types/)

{% include links.html %}