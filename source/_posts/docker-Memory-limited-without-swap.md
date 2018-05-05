---
title: docker出现Memory limited without swap
tags:
  - ubuntu
  - docker
categories:
  - ubuntu
abbrlink: 2808712848
date: 2018-05-05 13:35:09
---

ubuntu下docker出现 `WARNING: Your kernel does not support swap limit capabilities or the cgroup is not mounted. Memory limited without swap.`
<!-- more -->
### 解决办法
编辑 `/etc/default/grub` 文件，找到 `GRUB_CMDLINE_LINUX`,修改为
``` 
GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
```
执行 `sudo update-grub` ,然后重启电脑