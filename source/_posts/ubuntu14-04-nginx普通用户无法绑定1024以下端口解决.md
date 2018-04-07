---
title: ubuntu14.04 nginx普通用户无法绑定1024以下端口解决
tags:
  - ubuntu
  - nginx
categories:
  - nginx
abbrlink: 280768137
date: 2018-04-05 19:49:03
---

执行下面这条命令
<!-- more -->
``` 
sudo setcap 'cap_net_bind_service=+ep' /usr/local/nginx/sbin/nginx﻿
```