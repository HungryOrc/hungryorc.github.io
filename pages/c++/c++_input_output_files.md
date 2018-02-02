---
title: "Input/Output from/to Files in C++"
tags: [c++]
keywords: iostream, ofstream, ifstream, fstream, input, output, file, c++
summary:
sidebar: mydoc_sidebar
permalink: c++_input_output_files.html
folder: c++
toc: false
---

C++ provides the following classes to perform input/output of characters from/to files:
* `ofstream` (output file stream): stream class used to write to files
* `ifstream` (input file stream): stream class used to read from files
* `fstream` (file stream): stream class used to both read and write from/to files

These classes are derived directly or indirectly from the classes `istream` and `ostream`. We have already used objects whose types were these classes: `cin` is an object of class `istream` and `cout` is an object of class `ostream`. We can use our file streams the same way we are already used to use `cin` and `cout`, with the only difference that we have to associate these streams with physical files:
```c++
#include <iostream>
#include <fstream>
using namespace std;

int main () {
  ofstream myFile; // output file stream
  myFile.open ("example.txt");
  myFile << "Writing this to a file.\n";
  myFile.close();
}
```

This code creates a file called "example.txt", and inserts a sentence "Writing this to a file.\n" into it in the same way we are used to do with `cout`, but using the file stream `myFile` instead.

## Open a File

The first operation generally performed on an object of one of these classes is to associate it to a real file. This procedure is known as to open a file. An open file is represented within a program by a stream, and **any input or output operation performed on this stream object will be applied to the physical file associated to it**. 

To open a file with a stream object, we use the **member function** `open` **of the stream class**:
```c++
open (filename, mode);
```
`filename` is a string representing the name of the file to be opened, `mode` is an optional parameter with a combination of the following tags:

| flag | description |
|:-----|:------------|
| `ios::in` | Open for input operations. |
| `ios::out` | Open for output operations. |
| `ios::binary` | Open in binary mode. |
| `ios::ate` | Set the initial positoin at the end of the file. If this flag is not set, the initial position is the beginning of the file.
| `ios::app` | All output operations are performed at the end of the file, appending the content to the current content of the file.
| `ios::trunc` | If the file is opened for output operations and the file already existed, its previous content is deleted and replaced by the new content.

All these flags can be combined using the bitwise operator "OR" `|`, for example, if we want to open the file "example.bin" in binary mode, and to add data to it, we can do it like this:
```c++
ofstream myFile;
myFile.open ("example.bin", ios::out | ios::app | ios::binary);
```
Each of the `open` functions of classes `ofstream`, `ifstream` and `fstream` has a default mode to use if the file is opened without specifying any second argument:

| class | default mode parameter |
|:------|:-----------------------|
| `ofstream` | `ios::out` 
| `ifstream` | `ios::in`
| `fstream` | `ios::in | ios::out`

For `ifstream` and `ofstream` classes, `ios::in` and `ios::out` are automatically assumed, even if a mode without `ios::in`/`ios::out` has been specified as the second argument of function `open`, `ios::in`/`ios::out` will still be automatically combined with the flags that already existed in the mode expression.

For `fstream`, the default value is applied only if the function is called without any mode parameter. If the function is called with any value in that parameter the default mode is overridden, not combined.

File streams opened in **binary mode perform input and output operations independently of any format considerations**. Non-binary files are know as text files, and some translations may occur due to formatting of some special characters, like newline and carriage return.

Since the first task performed on a file stream is uaually to open a file, these 3 classes include a constructor that automatically calls the `open` member function and has the exact same parameters as this member function. So we could have declared the former `myFile` object and conduct the same opening operation by:
```c++
ofstream myFile ("example.bin", ios::out | ios::app | ios::binary);
```
Combining object costruction and stream opening in a single statement like the above.

To check if a file stream was successful on opening a file, you can call the member function `is_open()`, like: 
```c++
if (myFile.is_open()) { // ... }
```
It returns a `bool` of value `true` if the **stream object** is successfully **associated with an open file**.

## Closing a File

When done with a file we should CLOSE it so the operating system is notified and its resources become available again. For this we call the stream class' member function `close`, which flushes the associated buffers and closes the file:
```c++
myFile.close(); // myFile is a stream
```
Once this member function is called, the stream object can be re-used to open another file, and the file is available again to be opened by other processes. In case that a stream object is destroyed while still associated with an open file, the Destructor automatically calls its `close` function.

## Text Files

Text file streams are those where the `ios::binary` flag is NOT included in their opening mode. These files are designed to store text, thus all values that are input or output from/to them can suffer some formatting transformations, which do not necessarily correspond to their literal binary value. Writing operations on text files are performed in the same way we operated with `cout`:
```c++
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    ofstream myFile ("example.txt"); // output file stream
    if (myFile.is_open()) {
        myFile << "This is a line.\n"; // 这个语法就像用 cout 一样
        myFile << "This is another line.\n";
        myFile.close();
    } else {
        cout << "Unable to open file.";
    }
}
```

Reading from a file can also be performed in the same way as `cin`:
```c++
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main () {
  string line;
  ifstream myFile ("example.txt"); // input file stream
  if (myFile.is_open()) {
    while (getline (myFile, line)) {
      cout << line << '\n';
    }
    myFile.close();
  } else {
    cout << "Unable to open file";
  }
}
```
This code reads a text file and prints out its content on the screen. We have created a while loop that reads the file line by line, using `getline`. The value returned by `getline` is **a reference to the stream object itself**, which, when evaluated as a boolean expression, is `true` if the stream is ready for more operations, and `false` if either the end of the file has been reached or if some other error occurred.

## Checking State Flags

The following member functions of the stream class can check specific states of a stream, all of them return a `bool` value.

* `bad()`

  Returns `true` if a reading or writing operation fails. For example, in the case that we try to write to a file that is not open for writing or if the device where we try to write has no space left.

* `fail()`
  
  Returns `true` in the same cases as `bad()`, but also in the case that a format error happens, like when an alphabetical character is extracted when we are trying to read an int.

* `eof()` (end of)

  Returns `true` if a file opened for reading has reached the end.

* `good()`

  It is the most generic state flag: it returns `false` in the cases in which any of the previous functions return `true`. `good` and `bad` are not exact opposites, `good` checks more state flags.

* `clear()`

  Reset the state flags.

## Get and Put Stream Positioning

All I/O stream objects keep at least one internal position. `ifstream`, like `istream`, keeps an internal `get` position with the location of the element to be read in the next input operation. `ofstream`, like `ostream`, keeps an internal `put` position with the location where the next element has to be written. `fstream`, like `iostream`, keeps both the `get` and `put` positions.

These internal stream positions point to the locations within the stream where the next reading or writing operation is performed. These positions can be observed and modified using the following member functions:

### `tellg()` and `tellp()`

These two member functions with no parameters return a value of the member type `streampos`, which is a type representing the current `get` position in the case of `tellg`, or the `put` position in the case of `tellp`.

### `seekg()` and `seekp()`

These functions can change the location of the `get` and `put` positions. Both are overloaded with 2 different prototypes. The first form is:
```c++
seekg (position);
seekp (position);
```
Using this prototype, the stream pointer is changed to the absolute position `position`, counting from the beginning of the file. The type for this parameter is `streampos`, which is the same type as those returned by functions `tellg` and `tellp`.

The other form for these functions is:
```c++
seekg (offset, direction);
seekp (offset, direction);
```
Using this prototype, the `get` or `put` position is set to an offset value relative to some specific point determined by the parameter `direction`. `offset` is of type `streamoff`. And `direction` is of type `seekdir`, which is an enumerated type that determines the point from where offset is counted from, and that can take any of the following values:

| flag | description |
|:-----|:------------|
| `ios::beg` | offset counted from the beginning of the stream
| `ios::cur` | offset counted from the current position
| `ios::end` | offset counted from the end of the stream

The following example uses the member functions we have just seen to obtain the size of a file: 

```c++
#include <iostream>
#include <fstream>
using namespace std;

int main () {
  streampos begin, end;
  ifstream myfile ("example.bin", ios::binary);
  begin = myfile.tellg();
  myfile.seekg (0, ios::end);
  end = myfile.tellg();
  myfile.close();
  cout << "size is: " << (end - begin) << " bytes.\n"; // "size is: 40 bytes."
}
```









## Reference

* [Input/Output with Files [cplusplus.com]](http://www.cplusplus.com/doc/tutorial/files/)

{% include links.html %}