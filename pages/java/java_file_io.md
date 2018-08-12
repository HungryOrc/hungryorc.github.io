---
title: "File I/O in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_file_io.html
folder: java
toc: false
---
There are multiple ways to do file I/O in java. The following is a popular way.

## Read txt file
```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.FileNotFoundException;

...
// this is a relative file path,
// "docs" is a folder in the root directory of the Java project
String sourceFilePath = "docs/doc1.txt";
String targetFilePath = "docs/doc2.txt";

try {
    FileReader fileReader = new FileReader(sourceFilePath);
    BufferedReader bufferedReader = new BufferedReader(fileReader);
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

## Reference
* [Copy files with readers and buffers [LinkedIn Learning]](https://www.linkedin.com/learning/java-essential-training-objects-and-apis/copy-files-with-readers-and-buffers)


{% include links.html %}
