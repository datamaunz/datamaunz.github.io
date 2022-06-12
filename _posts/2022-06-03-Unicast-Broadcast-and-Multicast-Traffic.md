--- 
layout: post 
title: Unicast, Broadcast and Multicast Traffic
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-4-OSI-Layer-3-Network-Layer
tags: [ccna, networking, network layer, ip, ipv4]
---

## Overview

- 3 main IP traffic types
    - **unicast**: to a single destination host
        - to send to multiple hosts, separate copies of the same traffic have to be sent to each host individually
    - **broadcast**: to all hosts on the subnet
        - one copy of the traffic is sent everywhere throught part of a particular network
        - routers **do not** forward broadcast traffic
    - **multicast**: to multiple interested hosts
        - one copy that goes to multiple different destinations
        - in contrast to broadcast, it is targeted (that means the receiver has to request it in order to get it)
        - ***metaphor***: think of a radio station; those who want to receive it have to tune into the frequency
