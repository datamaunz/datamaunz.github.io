--- 
layout: post 
title: Class A IP Addresses
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-5-IP-Address-Classes
tags: [ccna, networking, class A, IP, IPv4]
---

## subnet size

- tradeoff between # of networks and # of hosts
- the bigger the host portion of the network, the more hosts are possible but the fewer networks
    - e.g., if the subnet mask is
        /8, 24 bits available to allocate hosts
        /24, 8 bits available to allocate hosts


## Idea behind internet addressing

- `IANA (Internet Assigned Numbers Authority)`: responsible for global coordination of internet IPv4 addressing
- original idea: 
    - company wants to communicate on the internet
        - applies for a range of IP addresses
    - if they have 6,000 hosts, they ask for a range of IP addresses to cover that (factoring in some growth)
    - allocation of addresses to their hosts in various offices

### Problem with IPv4

- internet grew much bigger
- IANA ran out of IP addresses
- **long term solution** is IPv6 (128 bits)
- **intermediary soultion**: private IP addresses with NAT (Network Address Translation)

## Class A

- internet authorities split IPv4 address space into classes
- `class A`: assigned to networks with a very large number of hosts
    - the **higher order** (first) bit in a class A address **is set to zero**
    - default subnet mask is /8
    - valid network addresses range from 1.0.0.0 to 126.0.0.0/8
        - allows for 126 networks and 16,777,214 (2^24) hosts
        - e.g. 15.0.0.0/8
    - reserved Class addresses (unnecessarily block 33,554,428 addresses from the global address pool):
        - 0.0.0.0/8 signifies **this network**
            - 0.0.0.1 to 0.255.255.255 are not valid host addresses
        - 127.0.0.0/8 in the Class A space is reserved as the loopback address for testing the local computer
            - 127.0.0.1 to 127.255.255.255 are not valied host addresses

### Subnetting

- putting all 16,777,214 hosts into a single logical network would result in **poor performance** and **poor security**
    - instead the company could split their /8 address allocation into smaller subnets
    - the subnets could then be allocated to different offices and types of hosts
    - e.g. they receive 15.0.0.0/8
        - 15.0.1.0/24 could be allocated to sales computers in New York,
        - 15.0.2.0/24 could be allocated to accounting PCs,
        - 15.0.9.0/24 could be allocated to sales computers in Boston,





