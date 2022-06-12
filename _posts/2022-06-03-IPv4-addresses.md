--- 
layout: post 
title: IPv4 Addresses
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-4-OSI-Layer-3-Network-Layer
tags: [ccna, networking, binary, counting]
---

## Fundamentals

- an IPv4 address is 32 bits long
- written as 4 *octests* in dotted decimal format
- for example: 192.168.10.15
- each octet is 8 bits long: (4 x 8 = 32 )



```
# check ip on windows command prompt
ipconfig
```

```
# check ip on linux terminal
ifconfig
```

```
# get default gateway on linux terminal
ip route
```

```
# check ip in Cisco IOS

enable
ip interface brief # show ip address
show interface # more detailed with subnet mask etc.
```

## Static vs. automatic addressing

- IP address usually 
    - **set manually** on servers, printers, and network devices
    - **assigned automatically** on desktop computers using DHCP (**D**ynamic **H**ost **C**onfiguration **P**rotocol)
- logical separation between subnets can be explained in terms of the IP address (in binary)

## IPv4 Address Octets

- each octet in IP address has a value between 0 and 255

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |

- let's convert 192.168.10.15

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 (64) | 1 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| 1 (40) | 0 (40) | 1 (8) | 0 (8) | 1 (0) | 0 (0) | 0 (0) | 0 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (10) | 0 (10) | 0 (10) | 0 (10) | 1 (2) | 0 (2) | 1 (0) | 0 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (15) | 0 (15) | 0 (15) | 0 (15) | 1 (7) | 1 (3) | 1 (1) | 1 (0) |

11000000.10101000.00001010.00001111

## Subnet masks

- to set boundary between logical networks (subnets), the IP address is combined with a subnet mask