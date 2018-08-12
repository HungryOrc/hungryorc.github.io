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
There are multiple ways to do file I/O in java.

## Read and write txt file in traditional way, with `FileReader/Writer` and `BufferedReader`
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

## Read and write txt file with `Path`, starting with Java 7
`Path` in Java represents either a file or a directory. Using `Path` to do the file I/O is more convenient than the previous way.
```java
import java.nio.file.Path;
import java.nio.file.Paths;

...
// in get(...), 
// the last parameter is the file name, including the extension.
// the previous parameters represent the relative path starting from the root directory of
// the Java project, not including the file name, each of these strings represents a folder name.
// so the following parameters represents: "docs/subFolder/subsubFolder/doc1.txt":
Path sourceFilePath = Paths.get("docs", "subFolder", "subsubFolder", "doc1.txt");
```


## Reference
* [Copy files with readers and buffers [LinkedIn Learning]](https://www.linkedin.com/learning/java-essential-training-objects-and-apis/copy-files-with-readers-and-buffers)


{% include links.html %}
