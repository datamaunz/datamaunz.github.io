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



