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
### L2 and MAC (Media Access Control)
L2 Contains:
* Destination MAC
* Source MAC
The Ethernet Standard of IEEE specifies MAC Addresses to be used for the source and destination address of each L2 frame.

A MAC address is 48 bits (6 bytes) long. Typically for human reading they're written in the following format: `00:11:22:AB:CD:EF`.
* The first 3 bytes of the MAC (the OUI) is assigned to a manufacturer of network equipment by an organization called IANA (Internet Assigned Numbers Authority).
* The second 3 bytes is given out by that vendor and put on each network **interface**, not device. So one device can have multiple MAC, if it has multiple interfaces, for example a Router would have multiple interfaces (slots).
* MAC address is intended to be **globally unique**, although it is used for local addressing.

### L3 and IP (Internet Protocol)
Contains:
* Source IP 
* Destination IP

The Internet Protocol specifies that a 32 bit (4 byte) IP address should be written in the format of `11:22:33:44` for human reading. Each section is a decimal number between 0 and 255. 
* Some numbers/number-patterns are reserved for special purposes.
* Each IP address should be **globally unique** across the entire internet, with some exceptions.

## Addresses and Path Finding
Why do we have 2 kinds of Addresses contained in L2 and L3 respectively? 
* The MAC Addresses in L2 tell us where to go **next**, it's **Local Address**. It's frequently chagned in a Packet.
* The IP Addresses in L3 tell us where to go **ultimately**, it's **Global Address**. It's usually not changed in a Packet, some exceptions apply.

## How does a Switch and a Router parse the Packet
A packet is read by a device from its first bit to last bit as they are transmitted. The Packet's "headers" go first as they provide connection information for the "payload".

A Switch only looks at the L2 header.

A Router does the following in sequence:
* Looks at the L2 header, make sure the packet is destined for it, then removes the L2 header.
* Looks at the L3 header and manages to find out where to send the packet next.
* Based on the finding of the previous step, place a new L2 header back on the packet.
* Leaves the original L3 header unmodified in the packet.
* Some Routers can also look into the L4 header if confiured to do so.
  
For a Firewall:
* Firewalls typically look at at least the L4 header.
* Some advanced Firewalls inspect the Frame all the way to L7 (the Application Layer) to make decisions or to filter the Frame if necessary. 
  * The L7 portion of the packet is the portion that an Application on your computer cares most.
  



## Reference
* [Packets 101 [Youtube]](https://www.youtube.com/watch?v=4o3trxRk8Wg)

{% include links.html %}
