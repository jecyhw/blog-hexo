---
title: hexo 404公益
tags:
  - hexo
  - 404
categories:
  - hexo
abbrlink: 1924118775
date: 2018-04-06 21:12:24
---
在source文件夹下创建`404.html`，复制下面内容
<!-- more -->

``` 

---
layout: false
title: "404"
---
<!DOCTYPE HTML>
<html>
<head>
    <title>404</title>
    <meta name="description" content="404错误，页面不存在！">
    <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="robots" content="all" />
    <meta name="robots" content="index,follow"/>
</head>
<body>
<script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" charset="utf-8" homePageUrl="/" homePageName="回到我的主页"></script>
</body>
</html>
```

修改next主题`_config.yml`文件，去掉commonweal注释，如下所示
``` 
menu:
  commonweal: /404/ || heartbeat
```

