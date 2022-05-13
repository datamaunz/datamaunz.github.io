--- 
layout: post 
title: OSI Reference Model
subtitle: Basic Characteristics
categories: Overview
tags: [CCNA, Networking, OSI]
---

## OSI

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

| Layer | Name | Includes | Devices |
| --- | --- | --- | --- |
| 7 | Application |  |  |
| 6 | Presentation |  |  |
| 5 | Session |  |  |
| 4 | Transport | TCP/UDP, Port |  |
| 3 | Network | IP Address | Routers |
| 2 | Data Link | Ethernet MAC Address | Switches |
| 1 | Physical |  | Hubs |

- Upper Layers: 7,6,5
    - Application developers
- Lower Layers: 4,3,2,1
    - Network engineers

- Sender sends email
- the package goes from layer 7 down to 1
- and then up from 1 to 7 when arriving at receiver

Describing problems in terms of OSI model:

- A cable was unplugged: Layer 1 problem
- A user made a mistake: Layer 8 problem

## Accronyms to remember the names of the layers

- **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way
- **P**lease **D**on't **N**eed **T**hose **S**tupid **P**ackets **A**nyway
- **P**lease **D**o **N**ot **T**each **S**tudents **P**ointless **A**cronyms
- **P**lease **D**o **N**ot **T**ake **S**ales **P**eople's **A**dvice
- **P**lease **D**o **N**ot **T**ouch **S**uperman's **P**rivate **A**rea