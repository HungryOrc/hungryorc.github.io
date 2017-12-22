---
title: "Iterate Through Properties of Javascript Object"
tags: [javascript]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: javascript_iterate_properties.html
folder: javascript
toc: false
---

## Iterate by "for...of" and "Object.entries()"

```js
const obj = { a: 5, b: 7, c: 9 };
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
}
```

If the `value` is unneccessary, we can do it like:

```js
for (const [key, ] of Object.entries(obj)) {
```

## Iterate by "for...in"

Iterating over properties by `for...in` requires this additional hasOwnProperty check:

```js
for (var property in object) {
    if (object.hasOwnProperty(property)) {
        // ...
    }
}
```

It's necessary because an object's prototype contains additional properties for the object which are technically part of the object. These additional properties are inherited from the base object class, but are still properties of object.

hasOwnProperty simply checks to see if this is a property specific to this class, and not one inherited from the base class.

## Reference

* [Object.entries() in Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
* [Iterate Through Object Properties](https://stackoverflow.com/questions/8312459/iterate-through-object-properties)

{% include links.html %}