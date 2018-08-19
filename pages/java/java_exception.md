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

## Custom Exceptions
```java
// MyException.java
public class MyException extends Exception {
    // this member variable is required by Java for all kinds of Exceptions
    public static final long serialVersionUID = 42L;
    
    // this member method is also required
    public String getMessage() {
        return "Haha! You never saw this kind of Exception before right?";
    }
}
```
```java
// Main.java
import <package name>.MyException;
...
public class Main {
    public static void main(String[] args) {
        ...
        try {
            ...
        } catch (MyException e) {
            System.out.prinln(e.getMessage());
        }
    }
}
```



## Reference
* [The try-with-resources Statement [Oracle doc]](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)
* [Defining and throwing a custom exception [LinkedIn Learning]](https://www.linkedin.com/learning/advanced-java-programming/defining-and-throwing-a-custom-exception)

{% include links.html %}
