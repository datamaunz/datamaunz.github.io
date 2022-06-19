--- 
layout: post 
title: Class B and C IP Addresses
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-5-IP-Address-Classes
tags: [ccna, networking, class B, class C, IP, IPv4]
---

## Class B addresses

- `class B`: assigned to medium-sized to large-sized networks
    - the **two high order** bits in a class B address are set to **binary 1 0**
    - default subnet mask is /16
    - valid network addresses range from 128.0.0.0 to 191.255.0.0/16
        - allows for 16,384 networks and 65,534 hosts per network
        - in practice it would be further subnetted

## Class C addresses

- `class C`: assigned to small networks
    - the **three high order** bits in a class C address are set to **binary 1 1 0**
    - default subnet mask is /24
    - valid network addresses range from 192.0.0.0 to 223.255.255.0/24
        - allows for 2,097,152 networks and 254 hosts per network
        - could be allocated as if for real world network, or subnetted into smaller subnets

## Note on Private Addresses

- there are reserved Private Addresses in each class
- valid to be assigned to hosts but they are not routable on the public internet
- originally designed for hosts in a private network with no internet connectivity
    - **class A**: 10.0.0.0 to 10.255.255.255
    - **class B**: 172.16.0.0 to 172.31.255.255
    - **class A**: 192.168.0.0 to 192.168.255.255