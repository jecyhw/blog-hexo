---
title: ubuntu16.04设置python3默认
tags:
  - ubuntu
  - python
categories:
  - ubuntu
abbrlink: 2379415734
date: 2018-05-05 08:50:59
---
ubuntu16.04自带有python2和python3，如何将python3设置为默认。
<!-- more -->
执行下面两个命令即可
``` 
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150
```
如果要切换到python其他版本，执行
``` 
sudo update-alternatives --config python
```