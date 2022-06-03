--- 
layout: post 
title: The IP Header
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-4-OSI-Layer-3-Network-Layer
tags: [ccna, networking, network layer, ip, ipv4]
---

## Function

- routing packets to their destination
- ensure **Quality of Service**
    - one type of traffic might require better level of service than another
    - e.g., voice or video more sensitve to delay; thus, we might give it priority over email

## Protocols

- **IP (Internet Protocol)**: best known layer 3 protocol (focus here on **IPv4**)
    - alternatives:
        - ICMP (Internet Control Message Protocol)
        - IPSec
- connectionless with no acknowledgements at Layer 3
- **IP addressing**: **logical** addressing scheme (implemented at layer 3)
    - used to partition the overall network into smaller **subnets**
    - improves
        - *performance* (by keeping the traffic where it needs to be on as opposed to going everywhere)
        - *security* (well defined access control)
        - *troubleshooting* (problems can be easier identified)
    - in contrast, Layer 2 MAC addresses use one big flat addressing scheme

## IP Header

1. 4-bit version | 4-bit hdr length | Type of service | 16-bit total length (in bytes)
2. 16-bit identification (ID) | 3 bit flags | 13-bit fragment offset
3. 8-bit time to live (TTL) | 8-bit protocol | 16-bit header checksum
4. 32-bit source IP address
5. 32-bit destination IP address
6. Header options, in any (0-40 bytes)
7. Data (variable length)

- first row:
    - *4-bit version*: ip version 4 or 6
    - *4-bit hdr length*: 4 bit header length (it can be a different length since header options can be of different lengths)
    - *Type of service*: used for Quality of Service information (we can mark here what kind of traffic it is; based on this marking, better service can be provided to this kind of traffic)
    - *16-bit total length (in bytes)*: total length
- second row: used for fragment information to keep track of them (1,500 byte default maximum packet size in ethernet)
- third row:
    - *8-bit time to live (TTL)*: prevent routing loops (looping of packets that don't reach their destination)
        everytime a packet goes through a router, the router will decrement TTL field by 1; once it is down to 0, the router will drop the packet
    - *8-bit protocol*: specifies layer for transportation type (usually TCP or UDP)
    - *16-bit header checksum*: checks that packet has not been corrupted in transit
- fourth row:
    - *32-bit source IP address*: specifies where the packet came from
- fifth row:
    - *32-bit destination IP address*: specifies where the packet is going to
- sixth row:
    - *Header options, in any (0-40 bytes)*: place to put in additional information (not commonly used)
- seventh row:
    - *Data (variable length)*: the rest of the packet


    
