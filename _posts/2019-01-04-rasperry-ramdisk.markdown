---
layout: post
title:  "Raspberry Pi ramdisk"
date:   2019-01-04 16:14:00 +0200
categories: raspberry pi
---
Goal is to have a temporarily storage in ram to save tmp files.


Create a dir in the home: 

`sudo mkdir /home/pi/ramdisk` 

then edit the fstab file by

`sudo nano /etc/fstab`

and append the line

`tmpfs /home/pi/ramdisk tmpfs nodev,nosuid,size=50M 0 0` 

size=50M represent the ramdisk size. Change it as you prefer. Remember to not exceed.


Save, close the file and issue

`sudo mount -a`

Check if the operation succeeded by issuing

`df`

which should report a tmpfs with 50MB in /home/pi/ramdisk

```
pi@raspberrypi ~ $ df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/root       14989220 2335380  12005428  17% /
devtmpfs          378780       0    378780   0% /dev
tmpfs             383388       0    383388   0% /dev/shm
tmpfs             383388   10244    373144   3% /run
tmpfs               5120       4      5116   1% /run/lock
tmpfs             383388       0    383388   0% /sys/fs/cgroup
/dev/mmcblk0p1     58234   22148     36086  39% /boot
tmpfs              76676       0     76676   0% /run/user/1000
tmpfs              51200       0     51200   0% /home/pi/ramdisk
```
