---
title: "Best Practices in Javascript"
tags: [javascript]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: javascript_best_practices.html
folder: javascript
toc: false
---

## 命名

* require 一个 module 以后得到的指代这个 module 的变量，它的命名应与 module 名相同，或尽量相近。包括大小写字母的使用：
  ```js
  const fs = require("fs");
  ```

* 如果只需要一个 module 里的个别 methods，则用 deconstruct 的方式引用它们：
  ```js
  const { doThis, doThat, andAlsoDoThat } = require("myModule");
  ```
  特别注意！如果要用 sinon.stub 来 stub 一个 method，则不要用上述的方式！而是用 `const myModule = require("myModule");`。否则可能会报错无法 stub 本 method。


## Reference

* []()

{% include links.html %}