--- 
layout: post 
title: The Subnet Mask
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-4-OSI-Layer-3-Network-Layer
tags: [ccna, networking, binary, subnet, subnet mask]
---

## function

- *on the same subnet*, host can send traffic directly to each other
- to send traffic to a host on a different subnet, it must be forwarded by a router
- the `subnet mask`
    - enables the host to understand, if the destination is on the same or a different subnet
    - 32 bits long
    - written in dotted or slash notation

## network and host portion

- host's IP address consists of:
    - network portion and host portion
    - the subnet mask defines the boundary between both portions

### example

- host's IP address: 192.168.10.15
- subnet mask: 255.255.255.0


#### host IP in binary

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 (64) | 1 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 (40) | 0 (40) | 1 (8) | 0 (8) | 1 (0) | 0 (0) | 0 (0) | 0 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (10) | 0 (10) | 0 (10) | 0 (10) | 1 (2) | 0 (2) | 1 (0) | 0 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (10) | 0 (10) | 0 (10) | 0 (10) | 1 (7) | 1 (3) | 1 (1) | 1 (0) |

#### subnet mask in binary

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 (127) | 1 (63) | 1 (31) | 1 (15) | 1 (7) | 1 (3) | 1 (1) | 1 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 (127) | 1 (63) | 1 (31) | 1 (15) | 1 (7) | 1 (3) | 1 (1) | 1 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 (127) | 1 (63) | 1 (31) | 1 (15) | 1 (7) | 1 (3) | 1 (1) | 1 (0) |

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) | 0 (0) |

#### interpretation

- IP address is compared with the subnet mask
- `1`in the subnet mask: that bit in the IP address is part of the network address
- `0`in the subnet mask: the bit is part of the host address
- network address portion: 192.168.10
- host address portion: 15

#### consequences

- host wants to communicate with 192.168.10.20
    - same subnet --> traffic canbe sent directly
- host wants to communicate with 192.168.11.20
    - traffic has to be sent via router

## valid subnet masks

- always begins with contiguous array of **1s**#
    - valid: 11111111.11110000.00000000.00000000
    - invalid: 11111101.11110000.00000000.00000000

## host portion

- the `host portion` of the address is allocated to the different hosts on the subnet (e.g. PCs, servers, printers, router, interfaces, swithc management addresses)
- two exceptions:
    - all **0** in the host portion designates the network address
        - in our example, the network address is: 192.168.10.0
    - all **1** in the host portion designates the directed broadcast address for the subnet
        - traffic with this destination will be sent to all hosts in the subnet
        - in our example, the broadcast address is: 192.168.10.255

- host portion of the address must be **unique on the subnet**
- no need for sequential numbering
    - having a host with 10.10.10.10 and another with 10.10.10.20 is valid
- this leaves us with 254 possible host addresses in this subnet

## network address (network ID)

- the array of **0s** in the subnet mask signifies the network address
- all **0s** in the host portion is reserved for the network address
    - in our example, the network address is: 192.168.10.0

## slash notation

- subnet mask always begins with contiguous **1s**
     - this array is thus 1 to 32 bits long (from left to right)
     - this allows for slash notation
        - 255.255.255.0 is equal to /24 (which signifies 24 contiguous **1s**)
        - 255.255.0.0 is equal to /16 (which signifies 16 contiguous **1s**)
- used in conversations and on network diagrams
    - example 1: 
        - 192.168.10.15 255.255.255.0 or
        - 192.168.10.15/24
        - the network address is 192.168.10.0/24
    - example 2: 
        - 10.10.10.15 255.0.0.0 or
        - 10.10.10.15/8
        - the network address is 10.0.0.0/8
