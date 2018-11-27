---
layout: post
title:  "Low disk writing speed Raspberry Pi"
date:   2018-11-27 21:47:27 +0200
categories: raspberry pi
---
TL;DR If the writing speed on external HDD (or flashdrive) is too slow try this:   
* Open file `/etc/usbmount/usbmount.conf`
* Search the line `MOUNTOPTIONS="sync,noexec,nodev,noatime,nodiratime,uid=1000,gid=1000"`
* Remove the `sync` so the line must be `MOUNTOPTIONS="noexec,nodev,noatime,nodiratime,uid=1000,gid=1000"`
* Save and reboot


Then writing speed should be increased a lot.   


If you want test the actual writing speed, supposing your HDD is mounted in `/media/usb0`, use `dd if=/dev/zero of=/media/usb0/output bs=8k count=10k`