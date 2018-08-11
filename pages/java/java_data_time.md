---
title: "Date and Time in Java"
tags: [java]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: java_date_time.html
folder: java
toc: false
---

## Date and Time for Java 7 and before
```java
import java.util.Date;
import java.util.GregorianCalendar;
...
// current date and time
Date now = new Date();

// specified date and time
// 8 for the 2nd parameter, namely for month, will result in Sep,
// since the months in the old Java starts at Zero
GregorianCalendar gc = new GregorianCalendar(2018, 8, 23); 
Date d1 = gc.getTime(); // 9-23-2018

gc.add(GregorianCalendar.DATE, 3); // add 3 days. you can add by day/month/year...
Date d2 = gc.getTime(); // 9-26-2018


```

## Date and Time for Java 8 and after
```java

```

## Reference

{% include links.html %}
