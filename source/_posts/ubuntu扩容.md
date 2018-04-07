---
title: ubuntu扩容
tags:
  - ubuntu
categories:
  - ubuntu
abbrlink: 3164053385
date: 2018-04-05 15:29:01
---

### 直接挂载NTFS分区
<!-- more -->

1. 新建挂载点`sudo mkdir -p /windows/sda8`
    
2. 在/etc/fstab添加`/dev/sda8 /windows/sda8 ntfs uid=1000,auto,user,nls=utf8,umask=0027,exec   0 0`

3. 最后执行`sudo mount -a`就能在/windows/sda8文件夹下直接用这个分区

### 挂载ext4分区

1. 先要格式化成ext4,可以使用gparted工具,或者`sudo mkfs.ext4 /dev/sda8`

2. 新建挂载点`sudo mkdir /sda8`

3. 在/etc/fstab添加`/dev/sda8   /sda8 ext4 defaults  0  2`

4. 最后执行`sudo mount -a`

