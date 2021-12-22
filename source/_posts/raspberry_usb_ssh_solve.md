---
title: raspberry usb ssh solve
date: 2021-09-27 15:20:46
tags: 
  - docker 
  - sudo
categories:
  - docker
---

After you've done what @Barnaby Hewitt says then this issue may occur:

In my case the Ubuntu host machine could not connect to the Pi Zero via SSH because the Ubuntu machine failed to retrieve an IP address. In order to fix that I set static IP addresses on both ends.

I mounted the SD card to the Ubuntu host machine and I created a file on the mounted SD card, ^path_to_mounted_sd^/rootfs/etc/network/interfaces.d/usb0 containing:

```
allow-hotplug usb0
iface usb0 inet static
address 192.168.137.2
netmask 255.255.255.0
network 192.168.137.0
broadcast 192.168.137.255
gateway 192.168.137.1
```

After saving the file and successfully un-mounting the SD card I put it in the Pi and plugged connected my Pi to my computer via a USB cable.

Ubuntu's network manager started to show this icon: enter image description here indicating that it tries to retrieve an IP using DHCP. In the end it will fail so we will need to provide a static IP address.

Next you will need to locate the static IP address of the Ubuntu host machine. List all interfaces in order to locate the one that does not yet have an IP address via the ifconfig command. Afterwards run:

`sudo ifconfig enp0s20f0u6 192.168.137.3 netmask 255.255.255.0`
In my case enp0s20f0u6 is the interface that was located using the ifconfig command on the Ubuntu host. Then I could manage to connect using the command:

`ssh pi@192.168.137.2`
Because I did not change the default password yet I used raspberry to connect to the Pi.

In case you plug the Pi to the very same USB then you may want to have these network settings remain the same. For the configuration use the Ubuntu's /etc/network/interfaces or use the network manager in order to set these settings.
