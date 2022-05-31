--- 
layout: post 
title: OSI Reference Model
subtitle: Basic Characteristics
categories: Overview
tags: [CCNA, Networking, OSI]
---

## Basics

> Open Systems Interconnect Model

- ISO standard
- standardizes how computer communicate over a network
- 7-layered approach to data transmission
    - operations are divided into related groups of actions at each layer
- a layer serves the layer above it

Example

Sending an email:

- Computer (sender)
- Switch
- Serve (receiver)

## Layers (Overview)

| Layer | Name | Includes | Devices |
| --- | --- | --- | --- |
| 7 | Application |  |  |
| 6 | Presentation |  |  |
| 5 | Session |  |  |
| 4 | Transport | TCP/UDP, Port |  |
| 3 | Network | IP Address | Routers |
| 2 | Data Link | Ethernet MAC Address | Switches |
| 1 | Physical |  | Hubs |

- Sender sends email
- the package goes from layer 7 down to 1
- and then up from 1 to 7 when arriving at receiver

Describing problems in terms of OSI model:

- A cable was unplugged: Layer 1 problem
- A user made a mistake: Layer 8 problem

### Accronyms to remember the names of the layers

- **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way
- **P**lease **D**on't **N**eed **T**hose **S**tupid **P**ackets **A**nyway
- **P**lease **D**o **N**ot **T**each **S**tudents **P**ointless **A**cronyms
- **P**lease **D**o **N**ot **T**ake **S**ales **P**eople's **A**dvice
- **P**lease **D**o **N**ot **T**ouch **S**uperman's **P**rivate **A**rea

## Upper Layers

- Upper Layers: 7,6,5
    - Application developers

### Layer 7: Application Layer

- provides network services to the application of the user
- establishes availability of intended communication partners
- agreement on procedures for error recovery and control of data integrity

### Layer 6: Presentation Layer

- ensures information sent at layer 7 of system 1 is readable by layer 7 of another system
- translates among multiple data formats using a comon format
- e.g. computers with different encoding schemes

### Layer 5: Session Layer

- establishes, manages, and terminates sessions between two communicating hosts
- synchronizes dialog between presentation layers of the two host and manages their data exchange
    - e.g., web servers have many users (many communication processes open at any given time to track)
- offers efficient data transfer, CoS, and exception reporting of upper layer problems

## Lower Layers

- Lower Layers: 4,3,2,1
    - Network engineers

### Layer 4: Transport Layer

- main characteristics: TCP (preferrable for reliability) or UDP (preferrable for speed) transport used? What is the port number?

Definition:
- defines services to
    - segment,
    - transfer, and
    - reassemble the datafor individual communications between the end devices
- breaks down large files into smaller segments taht are less likely to incur transmission problems

### Layer 3: Network Layer

- most important info: the source and destination IP address
- the layer where routers operate

Definition:
- provides connectivity and path selection between two host systems (they might be located on geographically separated networks)
- manages the connectivity of hosts by providing logical addressing

### Layer 2: Data-Link

- most important info: source and destination layer 2 address
    - e.g., the source and destination MAC address if Ethernet is the layer 2 technology
- the layer where switches operate

Definition:
- defines
    - how data is formateed for transmission and
    - how access to physical media is controlled
- typically includes error detection and correction to ensure a reliable delivery of the data

### Layer 1: Physical Layer

- physical components of the network, e.g., the cables being used

Definition:
- enables bit transmission between end devices
- defines specifications needed for 
    - activating, 
    - maintaining, and 
    - deactivating the physical link between end devices
- e.g., voltage levels, physical data rates, maximum transmission distances, physical connectors etc.