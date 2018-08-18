---
title: "Exception Handling in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_exception.html
folder: java
toc: false
---

## Try-With-Resources Statement
"Try-With-Resources" is an improved style of try catch block in Java. 用了它以后，好处是：
* 不用 close 你在 try block 里定义的任何东西，比如各种 FileReader, BufferedReader, FileWriter...
* 不用为了在 try 和 catch 两个 block 里都能访问到某些变量，而把这些变量定义在 try block 的前面（外面）

举例如下
```java
try (
    FileReader fr = new FileReader(fileName);
    BufferedReader br = new BufferedReader(fr);
) {
    String curLine;
    while (curLine = br.readLine() != null) {
        System.out.println(curLine);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

## Reference
* [The try-with-resources Statement [Oracle doc]](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

{% include links.html %}
