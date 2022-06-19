--- 
layout: post 
title: CIDR Classless Inter-Domain Routing
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-6-Subnetting
tags: [ccna, networking, CIDR, route summarisation]
---

## Problem with IP Address Classes

- companies would receive class A (16,777,214 hosts), B (65,534 hosts) or C (254 hosts) classes depending on the number of hosts needed
- jumps between allocatable hosts within these classes are huge
    - thus, huge amount of global address space was wasted
- solution: **Classless Inter-Domain Routing (CIDR)**

## CIDR as partial solution

- `CIDR`: removed /8, 16/ and /24 requirements for the address classes
    - allowing for them to be split or **subnetted** into smaller networks
        -  e.g. 175.10.10.0/20
    - thus, address ranges can be allocated which more closely match existing needs reducing the waste of address space
- `Route Summarisation`: aggregate blocks of networks can be advertised on the internet
    - let's say
        - ISP A (internet service provider) gives out 256 address blocks (175.10.0.0/24, 175.10.1.0/24, 175.10.2.0/24, ..., 175.10.255.0/24)
        - ISP B gives out another 256 address blocks (175.11.0.0/24, 175.11.1.0/24, 175.11.2.0/24, ..., 175.11.255.0/24)
        - ISP A and ISP B get connected
        - without CIDR, ISP A would advertise all 256 address blocks to ISP B and vice versa
    - route summarisation allows to advertize an aggregate block
        - ISP A can advertise 175.10.0.0/16 to ISP B
        - ISP B can advertise 175.11.0.0/16 to ISP A
    - each ISP only needs to know the one summary route rather than 256 individual routes
        - reduces each ISP's routing table and takes up less memory
            - no need to recalculate routing tables if a link goes down in the other ISP's network