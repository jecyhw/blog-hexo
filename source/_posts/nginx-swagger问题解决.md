---
title: nginx swagger问题解决
tags:
  - nginx
  - spring boot
  - swagger
categories:
  - nginx
abbrlink: 1099619583
date: 2018-04-12 21:18:47
---
spring boot整合swagger打包之后使用nginx出错无法使用解决。
<!-- more -->

可尝试
``` 
upstream tomcat {
    server localhost:8080;
}

server {
    listen       80;
    server_name  localhost;
	root /usr/share/nginx/html/dist;

    location / {
        proxy_pass http://tomcat/;
        proxy_set_header Host $host; #指定host
    }
# ...
}
```

