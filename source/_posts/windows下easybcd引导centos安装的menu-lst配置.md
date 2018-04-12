---
title: windows下easybcd引导centos安装的menu.lst配置
tags:
  - windows
  - easybcd
  - centos
categories:
  - easybcd
abbrlink: 216010902
date: 2018-04-08 09:00:08
---

在windows下easybcd引导centos安装的menu.lst配置以及注意事项
<!-- more -->

### menu.lst配置
``` 
title Install CentOS
root (hd0,6)
kernel (hd0,6)/isolinux/vmlinuz linux repo=hd:/dev/sda7:/
initrd (hd0,6)/isolinux/initrd.img
```
> linux repo=hd:/dev/sda7:/表示centos的iso文件所在位置，hd:/dev/sda7:/“(序号从1开始)”和(hd0,6)“序号是0开始”指向的是同一个位置

### 安装centos时注意事项
centos可以识别etx3的磁盘格式（也可以识别fat32格式，但fat32格式的最大单个文件支持不能超过4g），无法识别ntfs，所以在安装前可以先用 EASEUS Partition Master工具来划分ext3分区，在将centos的iso镜像和image以及isolinux文件拷贝到这个分区下，再进行安装，最后在配置menu.lst
