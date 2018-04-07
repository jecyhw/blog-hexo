---
title: >-
  ubuntu configure: error: xml2-config not found. Please check your libxml2
  installation错误解决
tags:
  - ubuntu php
categories:
  - ubuntu
abbrlink: 3344135708
date: 2018-04-05 19:51:10
---

在ubuntu14.04上安装php7时，执行:./configure命令时
<!-- more -->
一直报
> configure: error: xml2-config not found. Please check your libxml2 installation。

使用`sudo apt-get install libxml2`显示这个已经安装

在网上查找后：需要安装libxml2-dev软件包才行

```
sudo apt-get install libxml2-dev
```