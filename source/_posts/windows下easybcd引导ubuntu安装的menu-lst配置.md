---
title: windows下easybcd引导ubuntu安装的menu.lst配置
tags:
  - windows
  - easybcd
  - ubuntu
categories:
  - easybcd
abbrlink: 736068404
date: 2018-04-08 09:12:51
---

 windows下easybcd引导ubuntu安装的menu.lst配置以及说明
 <!-- more -->
 
 ### menu.lst配置
 ``` 
 title Install Ubuntu
 root (hd0,0)
 kernel (hd0,0)/vmlinuz.efi boot=casper iso-scan/filename=/ubuntu-14.04.5-desktop-amd64.iso locale=zh_CN.UTF-8
 initrd (hd0,0)/initrd.lz
 title reboot
 reboot
 title halt
 halt
 ```
 ubuntu的iso文件位置在c盘，以及initrd.lz和vmlinuz.efi也拷贝到c盘，c盘在硬盘的位置可以从硬盘管理器查看(我的电脑->右键->管理)，序号从0开始
 (hd0,0)中hd0表示电脑第一块硬盘，0表示这块硬盘上的第一个分区(填ubuntu的iso文件所在分区位置)