---
title: ubuntu添加可信证书
tags:
  - ubuntu
categories:
  - ubuntu
abbrlink: 2735220453
date: 2018-04-05 15:10:26
---

### 添加证书
<!-- more -->
证书(扩展名为crt)复制到**/usr/local/share/ca-certificates**文件夹然后运行update-ca-certificates
``` 
sudo cp zhengshu.crt /usr/local/share/ca-certificates
sudo update-ca-certificates
```
### 删除证书
将**/usr/local/share/ca-certificates**对应的证书删除，然后执行update-ca-certificates
```
sudo rm -f /usr/local/share/ca-certificates/zhengshu.crt
sudo update-ca-certificates
```

参考链接:
> http://yaxin-cn.github.io/Linux/add-root-certificate-in-ubuntu.html