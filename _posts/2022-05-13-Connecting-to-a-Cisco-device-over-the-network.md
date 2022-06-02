--- 
layout: post 
title: Connecting to a Cisco Device over the Network (1)
subtitle: Using Putty
categories: Cisco-IOS-Operating-System
tags: [CCNA, Networking, Putty]
---

## Example

**Office 1**

- Local Area Network
    - [PC], [Server], [Server] -> switch
- Router


**Office 2**

- Local Area Network
    - [PC], [Laptop] -> switch
- Router


**Office 1** <--> Production Network <--> **Office 2**

- Assume you are at the PC in office 2 and you want to connect to the router in office 1 to change configurations.
- Use `Secure Shell` (SSH) to connect to its management IP address over the network
    - note that oyu could also use `Telnet`, but this protocol is insecure
    - commands entered in SSH are encrypted (as opposed to commands entered in Telnet)
- secure login typically enforced through integration with a centralised `AAA` server (**A**uthentication, **A**uthorization, **A**ccounting)

## Download and install Putty

- package for downloading can be found at [putty.org](https://putty.org/)
- for MacOS versions, see [here](https://www.ssh.com/academy/ssh/putty/mac)

## Connect to Router

- open Putty
- enter the IP address of the device you want to connect to
- enter your username and password
- now you're on the command line of your router and can start configuring it

## Management Network

In additon to the **Production Network** connecting *Office 1* and *Office 2* most companies have a **Management Network** connecting both offices. 

**Office 1** <--> Management Network <--> **Office 2**

In case there is a problem with the Production Network, the Management Network can be used as a backup

- connection via Production Network is called `ìn Band` (using the same path as normal staff does). Connection via Management Network is called `Out of Band`.