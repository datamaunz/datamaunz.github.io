--- 
layout: post 
title: Class D and E IP Addresses
subtitle: Notes from Neil Anderson's CCNA Course
categories: IP Address Classes
tags: [ccna, networking, class D, class E, IP, IPv4]
---

- classes A, B, and C include all the addresses which are valid to be assigned to hosts
- what about 224.0.0.0 to 255.255.255.255?

## Class D addresses

- `class D`: reserved for IP **multicast addresses**
    - the **four high order** bits in a class D address are set to **binary 1 1 1 0**
    - not allocated to hosts
    - no default subnet mask
    - valid addresses range from 224.0.0.0 to 239.255.255.255
 
 ## Class E addresses

- `class E`: experimental and reserved for **future use**
    - the **four high order** bits in a class D address are set to **binary 1 1 1 1**
    - not allocated to hosts
    - no default subnet mask
    - valid addresses range from 240.0.0.0 to 255.255.255.255
    - 255.255.255.255 is the broadcast address for *this network*
 

## IP address class summary

| Class | First Octet | default subnet mask (slash) | default subnet mask (dotted decimal) |
| --- | --- | --- | --- |
| A | 1 - 126 | /8 | 255.0.0.0 |
| B | 128 - 191 | /16 | 255.255.0.0 |
| C | 192 - 223 | /24 | 255.255.255.0 |
| D | 224 - 239 |  |   |
| E | 240 - 255 |   |   |

