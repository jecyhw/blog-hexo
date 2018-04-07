---
title: ubuntu apache2启用python-cgi
tags:
  - ubuntu
  - apache
  - python-cgi
categories:
  - ubuntu
abbrlink: 2740217651
date: 2018-04-05 15:17:17
---
<!-- more -->
### 安装apache2
<!-- more -->
```
sudo apt-get install apache2
```
### 在apache2.conf中添加
```
ScriptAlias /cgi-bin/ /var/www/cgi-bin/
<Directory "/var/www/cgi-bin/">
   AllowOverride None
   Options +ExecCGI
   Order allow,deny
   Allow from all
   AddHandler cgi-script .py
</Directory>
```
### 启用cgi
```
sudo a2enmod cgi
```
### 将python脚本放在/var/www/cgi-bin/目录下，并赋可执行权限
```
sudo chmod 755 *.py
```