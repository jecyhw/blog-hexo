---
title: ubuntu pydio在nginx上部署
tags:
  - ubuntu
  - pydio
  - nginx
categories:
  - pydio
abbrlink: 3766093411
date: 2018-04-05 15:18:45
---


### pydio依赖环境
<!-- more -->
#### php5.6安装
``` 
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php5.6  php5.6-curl php5.6-gd php5.6-mbstring php5.6-mcrypt php5.6-mysql php5.6-xml php5.6-xmlrpc php-xdebug php5.6-intl php5.6-fpm php5.6-dev php5.6-cli php5.6-common

```
可以通过sudo apt-cache search php5.6来查看php有哪些扩展可以安装

#### nginx安装
官网安装参考链接
> http://nginx.org/en/linux_packages.html
### pydio部署
pydio下载地址
> https://sourceforge.net/projects/ajaxplorer/

pydio部署参考链接
> https://pydio.com/en/docs/kb/system/installing-debiannginx

注意:和参考中有几点不一样的地方：
1. 由于nginx.conf配置文件中包含了/etc/nginx/conf.d/*.conf,所以只需要在/etc/nginx/conf.d/目录下添加pydio.conf配置文件，在该文件中进行server配置。
2. php版本和参考中使用的不一样，需要对应成自己安装版本即可。
3. 打开pydio网站首页时，会检测依赖的php扩展是否安装，如果出现warning(警告),可通过sudo apt-cache search php5.6来查找缺失扩展使用apt install进行安装，或者通过pecl进行安装。


#### 启动ssl需要生成ssl证书
生成证书：
``` 
mkdir -p /etc/ssl/localcerts
apt-get install openssl
openssl req -new -x509 -days 3650 -nodes -out /etc/ssl/localcerts/nginx_pydio.pem -keyout /etc/ssl/localcerts/nginx_pydio.key
```

添加权限：
``` 
chmod 600 /etc/ssl/localcerts/*
chown -R www-data:root /etc/ssl/localcerts
```

最后在pydio的配置文件添加
找到 listen 443 ssl;在该行下面添加
``` 
ssl_certificate     /etc/ssl/localcerts/nginx_pydio.pem;
ssl_certificate_key /etc/ssl/localcerts/nginx_pydio.key;
```
    