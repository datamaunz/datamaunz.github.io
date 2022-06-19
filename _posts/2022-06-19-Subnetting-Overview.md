--- 
layout: post 
title: Subnetting Overview
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-6-Subnetting
tags: [ccna, networking, subnetting]
---

## Scenario

- small business
    - 4 departments in 2 offices
- instead of purchasing 4 separate address ranges from the internet authorities, we buy one single range (less expensive): Class C 200.15.10.0/24
    - this range can be divided up further and be assigend to the different departments
- when subnetting, **we take some of the host addresses away and give them to the network portion of the address**
    - the more subnets, the fewer hosts on the individual subnets and vice versa

## Calculate the number of networks

- `formula`: 2^subnet-bits
    - example 1:
        - class C network uses a /28 subnet mask
        - thus, we have borrowed 4 bits from the default of /24 (28 - 24)
        - 2^4 = 16 available subnets
    - example 2:
        - class B network uses a /28 subnet mask
        - thus, we have borrowed 12 bits from the default of /16 (28 - 16)
        - 2^12 = 4,096 available subnets
- hosts on different subnets need to go via a router if they want to communicate with each other

## Calculate the number of hosts

- `formula`: 2^host-bits - 2
    - subtract 2, because the network and broadcast addresses cannot be assigned to hosts
    - example:
        - class C network uses a /28 subnet mask
        - thus 4 bits left for hosts (32 - 28)
        - 2^4 - 2 = 14

## Note on ip subnet-zero

- in the past, the formula for calculating the number of networks was **2^subnet-bits - 2** (because it was not allowed to use network bits of all 0's or 1's)
- however, there was no real need for that
 ```
 # override this limitation on a router (enabled by default)
 ip subnet-zero
 ```





