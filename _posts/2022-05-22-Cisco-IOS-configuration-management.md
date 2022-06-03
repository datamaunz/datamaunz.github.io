--- 
layout: post 
title: Cisco IOS configuration management
subtitle: running config & startup config
categories: CCNA-2-Cisco-IOS-Operating-System
tags: [ccna, networking, ios, running config, startup config]
---

## Running vs. startup config


```
config t # to go to global configruation mode
```
```
hostname Router1 # the change from the previous name "Router" to "Router1" takes effect immediately in running config
```

**Running config**: The config that is in effect right now
**Startup config**: The config that will be in effect once the device is (re)booted

- as long as changes have not been saved, they wil only afffect the running config (not the startup config)
    - to get back to the original config, reboot (as the last resort)

```
end # to get to privileged exec mode
```
```
copy run start # save running config to startup config
```

 
## Backup your config to same device (bad idea)

```
copy run flash:my_config #  my_config will be the filename
```
```
sh flash # to verify that my_config is in there
```
```
# make my_config the startup config

erase start # on older router the command would be wr erase; it erases the content of the respective file, in this case the startup config
copy flash:my_config start # copy the content of my_config to start 
```

## Backup your config to different device (good idea)

```
# copy running config to a tftp server

copy run tftp # provide the IP address after hitting enter and afterwards the filename
```

```
# view the content of the file
more flash:my_config 
```

## Config storage locations

- IOS operating system is stored in Flash
- Startup Config is stored in NVRAM
- Running Config is stored in RAM (loaded into RAM from startup config when device boots up)


