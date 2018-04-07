---
title: ubuntu14.04添加静态ip和dns
tags:
  - ubuntu
  - ip
  - dns
categories:
  - ubuntu
abbrlink: 3414172487
date: 2018-04-05 14:36:13
---

### 添加静态ip
<!-- more -->
修改/etc/network/interfaces文件
``` 
auto eth0 
iface eth0 inet static 
address 192.168.0.117 
gateway 192.168.0.1 
netmask 255.255.255.0 
network 192.168.0.0 
broadcast 192.168.0.25
```
### 添加dns
编辑/etc/resolvconf/resolv.conf.d/base，这个文件默认是空的
``` 
sudo vi /etc/resolvconf/resolv.conf.d/base
```
配置dns
```
nameserver 8.8.8.8 
nameserver 8.8.4.4
``` 
保存，然后执行
``` 
sudo resolvconf -u 
```
查看/etc/resolv.conf，最下面是否多了2行： 
```
nameserver 8.8.8.8 
nameserver 8.8.4.4
```

### 重启网卡
sudo /etc/init.d/network restart