---
title: Trailing Slash in the URL
tags: [web]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: web_url_trailing_slash.html
folder: web
toc: false
---

Traditionally, URLs that pointed to files did not include the trailing slash, while URLs that pointed to directories did include the trailing slash. This means that:

* http://webdesign.about.com/example/ is a directory
* http://webdesign.about.com/example is a file

This distinction speeds up page loading because the trailing slash immediately tells the web server to go to that example directory and look for the index.html or other whatever the default file may be (this is set on the server level,but 'index' is a pretty standard file default and most servers will already be configured that way).

When you go to a URL without the trailing slash, the web server looks for a file with that name. If it doesn’t find a file with that name, then it looks for a directory and looks for the default file in that directory. This is not a best practice, since the file name in question does not include a file extension.


If, perhaps, you had two files with the same name but different extensions, like logo.png and logo.svg, the browser would not know which to display? It is therefore better to always use the file extension instead of relying on the browser to guess at which file you wanted.

## Reference

* [The Basics of the Traling Slash](https://www.thoughtco.com/urls-ending-with-slash-3466509)

{% include links.html %}