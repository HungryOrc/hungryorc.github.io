---
title: Packet, IP, MAC, Address Resolution
tags: [web]
keywords: OSI, TCP, Packet, IP, MAC, Address Resolution
summary:
sidebar: mydoc_sidebar
permalink: packet_ip_mac_address_resolution.html
folder: web
toc: false
---

## OSI Model
| Layer | Example |
|:----|:----|
| L7 Application | HTTP, FTP, SMTP, SSH, Telnet, L7 Firewalls
| L6 Presentation | 
| L5 Session |
| L4 Transport | TCP, UDP, L4 Firewalls
| L3 Network | IP, Routers
| L2 Data Link | Ethernet, Switches, Hubs(outdated)
| L1 Physical | Hardware, Optical Fiber, Copper Wire(outdated)

## TCP/IP Model
| Layer |
|:----|
| L7 Application
| L4 Transport
| L3 Network / Internet
| L1 & L2, Network Access and Network Interface



## Packet
```
┌────────────────┬────────────────┬────────────────┬──────────────────────────────────────────┐
| Content for L2 | Content for L3 | Content for L4 |              Content for L7              |
└────────────────┴────────────────┴────────────────┴──────────────────────────────────────────┘
|<------------------- Header --------------------->|<---------------- Payload --------------->|
|<---------------------------------------- Frame -------------------------------------------->|
                 |<------------------------------- Packet ----------------------------------->|
                                  |<----------------------- Segment ------------------------->|
```
### L2 
Contains:
* Destination MAC
* Source MAC

### L3
Contains:
* Source IP 
* Destination IP

## Address and Path Finding
Why do we have 2 kinds of Addresses contained in L2 and L3 respectively? 
* The MAC Addresses in L2 tell us where to go **next**, it's **Local Address**. It's frequently chagned in a Packet.
* The IP Addresses in L3 tell us where to go **ultimately**, it's **Global Address**. It's usually not changed in a Packet, some exceptions apply.

## Reference

* [Packets 101 [Youtube]](https://www.youtube.com/watch?v=4o3trxRk8Wg)

{% include links.html %}
