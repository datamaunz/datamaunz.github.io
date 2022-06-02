--- 
layout: post 
title: Intial Connection to a Cisco Device
subtitle: Direct Connection via Console Cable
categories: Cisco IOS Operating System
tags: [ccna, networking, console cable]
---

## Console connection to router via cable

- Cisco devices tend to not have initial IP addresses
- we thus need to set one up (initial configuration) if we want to connect to them
- `console connection`: connect to the device on a lower level than IP (e.g. to do the initial configuration)

- connect to the console port on the router or switch via a `console cable` (DB9 connector - RJ45 connector)
    - gives low level direct access to the command line
- **problem**: laptops don't come with serial ports anymore (needed to connect the DB9 connector)
- **solution**: USB to Serial adapter
- newer Cisco systems use USB to Mini-USB

## Putty

- package for downloading can be found at [putty.org](https://putty.org/)
- for MacOS versions, see [here](https://www.ssh.com/academy/ssh/putty/mac)

- make sure you select the correct port
    - to find the right port use your *device manager* (make sure the driver for your cable is installed) and check the ports (usually com3)
- power on your router
- bootup takes a few minutes

## Use cases

- initial configuration
- IP address becomes unresponsive
- troubleshoot the bootup process
    - viewing a devices's bootup not possible via SSH since system must have booted already before the IP address goes live






