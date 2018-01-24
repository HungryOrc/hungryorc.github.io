---
title: "Array in Javascript"
tags: [javascript]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: javascript_array.html
folder: javascript
toc: false
---

## Array 的一些特别操作

```js
array.sort(); // sort

array = array.map(element => element.toLowerCase()); // all elements converted to lower case

array = Array.from(new Set(array)); // deduplicate with the help of Set

array = array.filter(element => (registeredElementName[element] !== undefined)); // keep the plugins that we recognize


```




## Reference

* [Javascript reference [mozilla.org]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/)
* [How to Remove Duplicates from JavaScript Array [codehandbook.org]](https://codehandbook.org/how-to-remove-duplicates-from-javascript-array/)

{% include links.html %}