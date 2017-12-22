---
title: "Set the Key of a Property by Variable"
tags: [javascript]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: javascript_set_property_key_by_variable.html
folder: javascript
toc: false
---

If we want to use a variable to set the key of a property, we need to wrap the variable with a pair of `[]`, or the key will be set by the name of the variable:

```js
const key = "hurrah";
const value = "manhattan";
console.log({ key: value }); // key: manhattan
console.log({ [key]: value }); // hurrah: manhattan
```

```js
const key = "hp";
let footman = {};
footman[key] = 500;
// or:
const key = "hp";
const footman = { [key]: 500 };
```


{% include links.html %}