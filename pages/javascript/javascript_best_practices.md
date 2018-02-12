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




## -----------------------------------------------------------------------


module.exports.checkTimeStampFormat = function(timeStamp) {
   const is17DigitNumber = timeStamp.match(/^\d{17}$/);

declare a RegExp expression as a constant outside of this function. 
This will pre-compile it so that each run of checkTimeStampFormat is more efficient.




chain if-else branches in Promise chain

https://stackoverflow.com/questions/33257412/how-to-handle-the-if-else-in-promise-then





+    groups.forEach(function(group) {
+        group.urls.forEach(function(url) {
+            if (!uniqueURLs.includes(url)) {

Change .includes() to .has(),
includes method is expensive since it has to scan entire array and you already have 2 loops 

running




+module.exports.getUniqueURLsFromGroups = function(groups) {
+    let uniqueURLs = [];
...

+        });
+    });
+
+    return uniqueURLs;

Change uniqueURLs into Set:
let uniqueURLs = new Set();
then:
change the return to Array.from(uniqueURLs)





Flags of fs.createWriteStream:

Flag option allows you to set different behaviour related to writing or overwriting files.

For example, when creating a WriteStream, if you pass the flag w it will overwrite the file if 

it exists (this is the default value), whereas if you use the flag r+, it will just modify the 

file if it exists as it actually open the files for reading and writing or will have an error 

if it doesnt exist.

Here is a lost of flags and there explanation:

'r' - Open file for reading. An exception occurs if the file does not exist.
'r+' - Open file for reading and writing. An exception occurs if the file does not exist.
'w' - Open file for writing. The file is created (if it does not exist) or truncated (if it 

exists).
'w+' - Open file for reading and writing. The file is created (if it does not exist) or 

truncated (if it exists).
'a' - Open file for appending. The file is created if it does not exist.
'a+' - Open file for reading and appending. The file is created if it does not exist.






make the functions easy to use, minimize the number of input arguments that the users have to 

input,
make some of the options as different methods, e.g.: logger.info(...), logger.error(...), ...
rather than: logger("info", ...)
make some of the options encapsulated in the methods, e.g.: if the user uses logger.info(), 

then 
it's automatically logged to the console not the local log file,
and the context of the log, namely the file path from where the log was invoked, will not be 

displayed





try to notify the user to avoid using some private functions, by naming the functions with 

prefix: "_functionName"




Normally we don't create variables that are used once





All data clean-ups or preparations should be done outside the app





only log "server is online" in the app.listen(PORT, () => ...);
since that's when the server is really online,
otherwise it will be a misleading message!




don't need to require twice for these, just do:
const { deleteGroupByTimeStamp, updateGroupTimeStamp } = require("../models/group.js");




My mistake was:

In the previous promise, i wrote: resolve({ uniqueUrls, timeStamp })

and in the subsequent promise, i wrote: function({ urls, timeStamp })
I forgot that the names are names of properties, not names of arguments, so the naming should 

not be changed.




database usually have its own timestamp when it saves a new object/document/entry inside 

itself. 
the timestamp might be embedded in the _id of the object.
for example, for mongodb:
// run this after the new group has actually been saved
let timestampOfANewlyCreatedGroup = ObjectId(newGroup._id).getTimestamp(); 
use it if possible, try not to define your own timestamp




const mongoose = require("mongoose");
mongoose.Promise = require("bluebird");

for the 1st require, it must be done in every module that uses mongoose,
but for the 2nd setting of "mongoose.Promise",
we only need to do it once per app, since the "require" function caches module.
So we can do it when we first create the database.



const logModule = require(process.env.LOGGER_MODULE_PATH);

NEVER under any circumstances do this - its a security violation!
never rely directly on user input
dont interpolate file path unless you sanitize the string

The user might set something wrong or malicious in his own environmental variables, 
then by the code above, the User can just require some vulnerable module

Change this logic for user to only set type of log "console" or "file",
then you determine which file to require internally.






get function / get endpoint only get data!!! it does not save anything !!!
if you want to save some outcome of a get/search command, do it via some put endpoint !!!
or do some data pouring / restore before the program begins!!! do not do it inside get 

function/endpoint!!!!




one function only do one thing!!! which is the thing that the function's name indicates!!! not 

a bit more !!!




get package called `dotenv`

create local `.env` file and set all variables there




in your server.js run `require("dotenv").load()`




that will load all variables from `.env` file into `process.env`




defaults should be in the code
environment variables are by most part optional


 for the user to set.
so your code must handle cases when the user of the app didn't define that env variable
so we do like this:

class FileLogger extends Logger {
    
    constructor() {
        super();
        this._logPath = "logs/";
    }

    /**
     * @param {String} logPath relative path to the folder that holds the log files
     */
    setLogPath(logPath) {
        this._logPath = logPath;
    }

    // ...


    /**
     * write log to a local *.log file
     */
    _createOrAppendLogFile(logContent) {
        this._logPath = process.env.LOG_PATH || this._logPath;

        // ...
    };
}





make global constants in each js file all UPPER_CASE, e.g.:
const MIN_DIGITS = 6;




Do not use eval! Big No-no in JavaScript




you can access any property/method on an object by name,
just use square brackets.
for example:
console["methodName"](arg1, arg2)
in which methodName could be "info", "warn", "error", "trace", "debug"




keep all interactions with environment variables in one place



If the name of a function / method / module is already readable enough, and 
it precisely describes what its bearer does,
then don't need to add any comment to it




check if a hashmap (object) contains an item (property)

example:
let map = { "apple": 1, "pear": 2, "peach": 3 }
we want to check if map contains "grape"

we can do it like:
if(map["grape"])
since if map doesn't include a property "grape", then map["grape"] will return "undefined", 
and "undefined" will be regarded as "false"

another way to do it: use the "in" syntax:
if ("grape" in map)




use a return statement to eliminate else statement, if possible, this way is believed to be 

faster

if (something) {
    do something;
    return;
}
do other things;




for unit tests, don't test the express routers, since it's express's responsibility to make 

sure they work well,
we should test the handler functions of the express routers.
Basically we test the logic that we wrote.




const a = require("../path/a.js")
a must have the same name as a.js !!!!!
if a is a snake shape, something like a-bc-de, than the const must be named in camle! like 

aBcDe




try not to put error messages strings into Singletons, Classes, etc unless it needs to be 

exposed



 - I was trying to send this message to an outside router, which can use it in the response 

message. 
 - Don't know yet how to do it in a better way
responses should not reveal too much, it can be used to exploit the service
You could create an Enum of some standard error messages that need to be used in multiple place



drop success logging otherwise app would be too chatty. We mostly need logging for failures and 

warnings.



when we need to log errors, best to log the full error, since we might need a stack trace, 

don't just log error.message



function convertTimeStampToDateObj(timeStamp) {
    timeStamp = timeStamp.toString();

It's a bad practice to overwrite the input argument.
Please use another variable for a string version of timeStamp.
This should avoid confusion when reading the code as well.




pay attention! some arguments are optional! and look into what will they automatically be 

replaced by!
for example, the function:
new Date(year, month, day, hour, minute...)
all the arguments starting from "day" are optional! they will be filled by zeroes if not 

specified by the user!


## ----------------------------------------------------------------------



## Reference

* []()

{% include links.html %}
