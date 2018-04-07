---
title: ubuntu压缩文件到windows上乱码问题
tags:
  - ubuntu
categories:
  - ubuntu
abbrlink: 2014681399
date: 2018-04-05 15:34:25
---
修改/etc/default/locale文件
<!-- more -->
``` 
LANG="zh_CN.UTF-8"
LC_NUMERIC="zh_CN.UTF-8"
LC_TIME="zh_CN.UTF-8"
LC_MONETARY="zh_CN.UTF-8"
LC_PAPER="zh_CN.UTF-8"
LC_NAME="zh_CN.UTF-8"
LC_ADDRESS="zh_CN.UTF-8"
LC_TELEPHONE="zh_CN.UTF-8"
LC_MEASUREMENT="zh_CN.UTF-8"
LC_IDENTIFICATION="zh_CN.UTF-8"
```
