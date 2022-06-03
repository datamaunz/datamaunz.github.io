--- 
layout: post 
title: The Transport Layer Header, TCP and UDP
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-3-OSI-Layer-4-Transport-Layer
tags: [ccna, networking, transport layer, ports, tcp, udp]
---

## Function

- provides transparent transfer of data between hosts
- responsible for end-to-end error recovery and **flow control**

## Fundamental Terms

- **flow control**: process of adjusting the flow of data from the sender such that the receiving host can handle all of it
    - **metaphor**: you speak slower if someone does not understand the language that well (etc.)
    - if sender sends too fast (e.g., because of faster connection than sender), the sender can signal back to slow it down
- **session multiplexing**: process that enables a host
    - to support multiple sessions simultaneously
    - to manage the individual traffic of streams over a single link
- **ports**: used to identify the upper port layer protocol
    - example:
        - HTTP: port 80, 
        - SMTP email: port 25, etc.
    - sender also adds a source port number to the Layer 4 header
    - source & destination port number can be used to track sessions

example: 

- sender sends web traffic to destination port 80 from source port 1500
- if the receiver wants to send traffic back, it will flip the port numbers for source and destination
- stateful firewalls use this feature to keep track of connections

Sender ----- DST: 80 SRC: 1500 ------> Receiver

Sender <---- SRC: 80 DST: 1500 ------- Receiver

## Protocols

most common protocols
- **TCP**: Transport Control Protocol
- **UDP**: User Datagram Protocol

| **TCP** | **UDP** |
| --- | --- |
| Transport Control Protocol | User Datagram Protocol |
| **connection oriented** (once connection is established, data can be sent bidirectionally over that connection) | **not** connection oriented (no handshake connection setup between hosts) |
| **sequencing** (ensures that segments are processed in the correct order and none are missing) | **no** sequencing |
| **reliable** (receiver sends acknowledgements back to sender; lost segments are resent based on missing acknowledgements)  | **not** reliable (receiver does not send back acknowledgements to sender)  |
| performs **flow control**  | **no** flow control |
| allows for error detection and recovery  | error detection and recovery has to be provided by upper layers|

### TCP Three-Way Handshake

To initiate connection,
1. sender sends synchronized message to receiver
2. receiver sends synchronized acknowledgement back
2. sender sends an acknowledgement

Sender ---------SYN---------> Receiver

Sender <------SYN-ACK-------- Receiver

Sender ---------ACK---------> Receiver

### TCP Header

at least 20 Bytes (5 x 32 Bits)

1. Source Port (16) | Destination Port (16)
2. Sequence Number (32)
3. Acknowledgement Number (32)
4. Header Length (4) | Reserved (6) | Code Bits (6) | Window (16)
5. Checksum (16) | Urgent (16)
6. Options (0 or 32 if Any)
7. Data (Varies)

### UDP Header

at least 8 Bytes (2 x 32 Bits)

1. Source Port (16) | Destination Port (16)
2. Length (16) | UDP checksum (16)
3. Data

### TCP vs. UDP

- TCP selected for traffic that requires reliability
- real-time applications (e.g. voice, video) use UDP to reduce overhead
- some applicaitons can use both TCP and UDP

| TCP | UDP | TCP and UDP |
| --- | --- | --- |
| FTP (21) | TFTP (69) | DNS (53) |
| SSH (22) | SNMP (161) |  |
| Telnet (23) |  |  |
| HTTP (80) |  |  |
| HTTPS (443) |  |  |
