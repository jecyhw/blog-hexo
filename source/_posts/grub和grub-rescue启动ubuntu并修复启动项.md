---
title: grub和grub-rescue启动ubuntu并修复启动项
tags:
  - ubuntu
  - grub
categories:
  - ubuntu
abbrlink: 314144721
date: 2018-04-05 15:32:22
---

### grub启动修复
<!-- more -->
先加载启动文件进入ubuntu
``` 
set root=(hd0,8); (可以通过root来查看硬盘分区,root (hd0,x)查看分区格式,找到ubuntu启动项所在分区)
kernel /boot/grub/i386-pc/core.img;
boot
```

    
### grub-rescue启动修复

同样的，先加载启动文件进入ubuntu
``` 
set root=(hd0,8); (可以通过root来查看硬盘分区,root (hd0,x)查看分区格式,找到ubuntu启动项所在分区)
set prefix=(hd0,8)/boot/grub
insmod /boot/grub/i386-pc/normal.mod #或者insmod normal或者insmod /grub/normal.mod. 一般来说，肯定有一个不会出错
normal
```

进入ubuntu后，执行下面两条命令就可以修复启动项了
``` 
sudo update-grub
sudo grub-install /dev/sda
```