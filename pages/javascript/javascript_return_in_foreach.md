---
title: "return" in "forEach()" works as "continue" in regular loops"
tags: [javascript]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: javascript_return_in_foreach.html
folder: javascript
toc: false
---

对于 `forEach()` loop，每一个 iteration 里面都相当于在运行一个 function，所以如果想停止当前的 iteration，到下一个 iteration 去，`continue` 是没用的，必须用 `return`。

对于一般的 loop 来说，可以用 `continue` 做到这一点。




## Reference

* [Go to “next” iteration in javascript forEach loop](https://stackoverflow.com/questions/31399411/go-to-next-iteration-in-javascript-foreach-loop/31399448)

{% include links.html %}