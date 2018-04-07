---
title: hexo生成tags链接
tags:
  - hexo
categories:
  - hexo
abbrlink: 3741088398
date: 2018-04-05 15:00:48
---

### 新建tags页面
<!-- more -->
``` 
hexo new page "tags"
```

### 编辑tags文件夹下的index.md
``` 
title: tags
date: 2018-04-05 14:58:23
type: "tags"
```
### 启用themes/next/_config.yml文件中的tags，如下:
```
menu:
  tags: /tags/ || tags
```
