---
title: ubuntu14.04 pfp-fpm开机启动
tags:
  - ubuntu
  - phpfpm
categories:
  - php
abbrlink: 45252325
date: 2018-04-05 15:25:46
---
ubuntu14.04下设置pfp-fpm开机启动
<!-- more -->
### php下载

下载php的源码，编译好就行，然后找到init.d.php-fpm.in文件，拷贝到/etc/init.d/目录下，名字为php-fpm

### 修改php-fpm文件
找到以下内容并修改
``` 
prefix=
exec_prefix=
php_fpm_BIN=/usr/local/php5.6/sbin/php-fpm
php_fpm_CONF=/usr/local/php5.6/etc/php-fpm.conf
php_fpm_PID=/usr/local/php5.6/var/run/php-fpm.pid﻿​
```

### 设置开机启动
```  
sudo update-rc.d  -f  php-fpm  defaults
sudo runlevel﻿​
```


 