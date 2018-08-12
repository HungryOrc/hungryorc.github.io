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

## Read and write txt file
```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.FileWriter;
import java.io.IOException;

...
// this is a relative file path,
// "docs" is a folder in the root directory of the Java project
String sourceFilePath = "docs/doc1.txt";
String targetFilePath = "docs/doc2.txt";

// put the 3 reader/writer in the "()" after the "try" before the "{}" is
// to make them to be closed automatically after the try-catch block,
// no matter there will be an Exception or not.
// this syntax is valid beginning with Java 7
try (
    FileReader fileReader = new FileReader(sourceFilePath);
    // buffered reader is to read the file a bit at a time
    BufferedReader bufferedReader = new BufferedReader(fileReader);
    
    FileWriter fileWriter = new FileWriter(targetFilePath);
) {
    while(true) {
        String line = bufferedReader.readLine();
        if (line == null) {
            break;
        }
        fileWriter.write(line + "\n");
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

## Reference
* [Copy files with readers and buffers [LinkedIn Learning]](https://www.linkedin.com/learning/java-essential-training-objects-and-apis/copy-files-with-readers-and-buffers)


{% include links.html %}
