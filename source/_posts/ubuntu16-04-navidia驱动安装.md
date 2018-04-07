---
title: ubuntu16.04 navidia驱动安装
tags:
  - ubuntu
  - navidia driver
categories:
  - ubuntu
abbrlink: 2796597353
date: 2018-04-05 19:47:35
---
安装navidia驱动
<!-- more -->
``` 
sudo apt-get purge nvidia-*
sudo apt-get autoremove
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-364﻿​
```