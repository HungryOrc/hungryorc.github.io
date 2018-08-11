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
import java.text.DateFormat;
...
// current date and time
Date now = new Date();

// specified date and time
// 8 for the 2nd parameter, namely for month, will result in Sep,
// since the months in the old Java starts at Zero
GregorianCalendar gc = new GregorianCalendar(2018, 8, 23); 
Date d1 = gc.getTime(); // 9-23-2018

// add 3 days. you can add by day/month/year...
gc.add(GregorianCalendar.DATE, 3);
Date d2 = gc.getTime(); // 9-26-2018

// format Date and Time
DateFormat f = DateFormat.getDateInstance(DateFormat.FULL);
System.out.println(f.format(d2)); // Sunday, September 26, 2018
```

## Date and Time for Java 8 and after
```java
import java.time.LocalDateTime;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
...
LocalDateTime ldt = LocalDateTime.now();
System.out.println(ldt); // 2018-10-05T12:29:45.890832456

// the new Java's month index starts at One, not Zero, that's great!
LocalDate ld = LocalDate.of(2018, 1, 20);
System.out.println(ld); // 2018-01-20

// format Date and Time
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("M/d/yyyy");
System.out.println(dtf.format(ld)); // 1/20/2018
```

## Reference

{% include links.html %}
