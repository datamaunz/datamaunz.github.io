--- 
layout: post 
title: TCP/IP Stack
subtitle: Basic Characteristics
categories: Overview
tags: [CCNA, Networking, TCP-IP]
---

## Comparison to 

> main protocol stack for computer operations

TCP: **T**ransmission **C**ontrol **P**rotocol
IP: **I**nternet **P**rotocol

| OSI | TCP/IP|
| --- | --- |
| conceptual | used to transfer data in production networks |
| 7 layers documented | 4 layers documented |
| better to be referenced in conversations with other network engineers |  |

<hr>

| OSI Model | TCP/IP Stack | Cisco TCP/IP Stack Definition |
| --- | --- | --- | 
| Application |  |  |  
| Presentation | Application | 'Represents data users, encodes and controls the dialog' | 
| Session |  |  | 
| Transport | Transport | 'Supports communication between end devices across a diverse network' | 
| Network | Internet | 'Provides logical adressing and determines the best path through the network' | 
| Data Link | Network Access | 'Controls the hardware devices and media that make up the network | 
| Physical |  |  | 

<hr>

### Terminology for communication between hosts

| TCP/IP Stack 1 | Term for Protocol Data Unit (PDU) | TCP/IP Stack 2 |
| --- | --- | --- | 
| Application | Data | Application |
| Transport | Segment | Transport |
| Internet | Packet | Internet |
| Network Access | Frame | Network Access |