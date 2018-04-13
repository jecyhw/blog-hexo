---
title: Enable File Watcher to compile SCSS to CSS
tags:
  - WebStorm
categories:
  - WebStorm
abbrlink: 958030140
date: 2018-04-13 09:25:33
---

WebStorm设置Enable File Watcher to compile SCSS to CSS
<!-- more -->
### 安装ruby
下载 `ruby` 并安装，下载地址为：
``` 
http://www.ruby-lang.org/en/downloads/
```

### 安装sass
``` 
gem install sass
```
如果出现下面错误
>  SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://api.rubygems.org/specs.4.8.gz)

更新下源，再重新安装
``` 
gem sources --remove https://rubygems.org/
gem sources -a http://rubygems.org/
```

### 配置FileWatcher
如下图所示，选择 `SCSS`  
![进入FileWatcher](Enable-File-Watcher-to-compile-SCSS-to-CSS\a1a3cf17.png)
`Program` 那进入 `ruby` 安装目录的 `bin` 目录下，选择scss.bat。（如果是Sass就选择sass.bat）
![配置scss.bat](Enable-File-Watcher-to-compile-SCSS-to-CSS\7ccbd08f.png)