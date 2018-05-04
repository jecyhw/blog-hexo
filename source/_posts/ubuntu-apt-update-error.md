---
title: ubuntu执行apt update出现Aborted (core dumped)错误
abbrlink: 2007401021
date: 2018-05-04 17:43:11
tags:
---
ubuntu执行sudo apt update出现下面错误
```
Aborted (core dumped)
Reading package lists... Done
E: Problem executing scripts APT::Update::Post-Invoke-Success 'if /usr/bin/test -w /var/cache/app-info -a -e /usr/bin/appstreamcli; then appstreamcli refresh > /dev/null; fi'
E: Sub-process returned an error code
```
<!-- more -->
解决方法
``` 
sudo pkill -KILL appstreamcli
wget -P /tmp https://launchpad.net/ubuntu/+archive/primary/+files/appstream_0.9.4-1ubuntu1_amd64.deb https://launchpad.net/ubuntu/+archive/primary/+files/libappstream3_0.9.4-1ubuntu1_amd64.deb
sudo dpkg -i /tmp/appstream_0.9.4-1ubuntu1_amd64.deb /tmp/libappstream3_0.9.4-1ubuntu1_amd64.deb
```
再重新执行
`sudo apt-get update`