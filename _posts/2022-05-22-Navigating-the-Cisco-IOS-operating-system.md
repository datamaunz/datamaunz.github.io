--- 
layout: post 
title: Navigating the Cisco IOS operating system
subtitle: Commandline
categories: Cisco IOS Operating System
tags: [ccna, networking, ios, commandline]
---

## IOS Command Hierarchy

**ios is not case sensitive**

### levels

- `hostname>`: User exec mode
    - not that often used: limited set of commands that can be entered here
    - `?` shows all the commands that are available on this level
        - `Enter` scrolls through line by line
        - `Space` scrolls through page by page
        - to define scrollback options, go to settings (if you are using Putty)
    - `enable` (or simply `en`): enter privileged exec mode
- `hostname#`: Privileged exec mode (`enable`)
    - `disable` (or simply `disa`): to drop back down to user exec mode
        - type `di?` to see all the commands starting with *di*
    - `show` (or simply `sh`): get information about the device (in point in time; `debug` gives output that will be updated in realtime)
        - `sh ?`: display all possible keywords for this command
            - hit `Ctrl-C` to break out of a command
            - hit `Ctrl-A` to get to the start of the line
            - hit `up arrow` to bring back previous commands
        - `sh aaa ?`: displays all available options after `sh aaa`
        - use `tab key`for command completion
    - `configure terminal` (or simply `conf t`): move to global configuration mode    
- `hostname(config)#`: Global configuration mode (`configure terminal`)
    - `do` in front of command like `show`to enable it (unless your in privileged exec mode)
    - `interface` followed by the respective interface: move to interface configuration mode
- `hostname(config-if)#`: Interface configuration mode (`interface x`)


### most common commands on router or switch

- `show ip interface brief`: shows all the interfaces on the device with their status and ip addresses
- `show running config`: shows entire running configuration on the device
    - `sh run int fast0/0`: show config for this particular interface
    - `sh run | begin hostname`: show config starting from where hostname is mentioned
    - `sh run | ?`: to see all the available options
        - **atributes behind piped commands are case sensitive since they are regular expressions**
         
### navigating the levels

- `enable`: move to privileged exec mode
`configure terminal`: move to global configuration mode
- `interface` followed by the respective interface: move to interface configuration mode
- `exit` drops back down a level
- `end` drops back to privileged exec mode from any level