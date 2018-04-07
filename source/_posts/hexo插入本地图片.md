---
title: hexo插入本地图片
tags:
  - hexo
  - hexo-asset-image
categories:
  - hexo
abbrlink: 809422878
date: 2018-04-05 19:37:28
---

### 修改_config.yml
<!-- more -->
找到post_asset_folder设置为true
```
post_asset_folder: true
```
### 安装hexo-asset-image
``` 
npm install https://github.com/CodeFalling/hexo-asset-image -- save
```

### 图片引用
每次使用`hexo new "page"`时，会生成`page.md`和`page`文件夹，`page`文件夹用来存放`page.md`中引用的图片。
使用markdown语法引用图片即可。
```
![demo](page\demo.png)
```
