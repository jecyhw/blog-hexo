---
title: ubuntu apt代理设置
tags:
  - ubuntu
  - apt-proxy
categories:
  - ubuntu
abbrlink: 1368529979
date: 2018-04-05 15:11:44
---
编辑/etc/apt/apt.conf文件
<!-- more -->
```
Acquire::http::proxy "http://<proxy>:<port>/";
Acquire::ftp::proxy "ftp://<proxy>:<port>/";
Acquire::https::proxy "https://<proxy>:<port>/";
```
出现以下错误时:
> Packages  server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none

在/etc/apt/apt.conf添加：
```
Acquire::https::packages.cloud.google.com::Verify-Peer "false";
Acquire::https::apt.dockerproject.org::Verify-Peer "false";
```