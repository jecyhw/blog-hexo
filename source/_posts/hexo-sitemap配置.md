---
title: hexo SEO配置
tags:
  - hexo
  - SEO
categories:
  - hexo
abbrlink: 3504545898
date: 2018-04-06 21:57:24
---
hexo next主题博客主动提交链接到百度和谷歌
<!-- more -->
### sitemap配置
安装sitemap插件
``` 
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```
修改next主题下的`_config.yml`
``` 
menu:
  sitemap: /sitemap.xml || sitemap
  
seo: true

baidu_push: true
```
修改hexo的`_config.yml`的url为博客地址，这里是
``` 
url: https://jecyhw.github.io
```

### 博客链接URL唯一(可选)
`hexo-abbrlink`插件安装
``` 
npm install hexo-abbrlink --save
```
修改hexo的`_config.yml`
``` 
#permalink: :year/:month/:day/:title/
permalink: :abbrlink/
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: dec    # 进制：dec(default) and hex
```

### 添加robots.txt
在`source`目录下创建`robots.txt`,复制下面内容，修改`Sitemap`地址为博客地址
``` 
User-agent: *
Allow: /
Allow: /categories/
Allow: /tags/
Allow: /archives/
Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://jecyhw.github.io/sitemap.xml
Sitemap: https://jecyhw.github.io/baidusitemap.xml
```

部署提交到github
``` 
hexo g -d
```

### 链接提交到百度站点
进入百度站长工具
> https://ziyuan.baidu.com/site/index

填写博客地址
![填写博客地址](hexo-sitemap配置\63673f90.png)
设置站点领域
![设置站点领域](hexo-sitemap配置\beb98c99.png)
验证网站，这里选择文件验证
![验证网站](hexo-sitemap配置\64d1375a.png)
下载验证文件，放入到source目录下，打开验证文件，在最上面加入
```

---
 layout: false
---
```
部署到github上
``` 
hexo g -d
```
完成验证。百度收录比较慢，需要等待好长一段时间才能搜索到。
### 链接提交到谷歌
进入google站点平台
> https://www.google.com/webmasters/tools/home?hl=zh-CN

添加站点
![添加站点](hexo-sitemap配置\e5fe3d7e.png)

验证网站
![验证网站](hexo-sitemap配置\01a599c2.png)
下载验证文件，放入到source目录下，打开验证文件，在最上面加入
```

---
 layout: false
---
```
部署到github上
``` 
hexo g -d
```
完成验证。

添加站点地图
![添加站点地图](hexo-sitemap配置\70e37bd4.png)
等几个小时就能在google上搜索到自己的博客了

### 添加nofollow标签(可选)
 修改`themes/next/layout/_partials/footer.swig`，对所有<a>标签加上 `rel="external nofollow"`
 ``` 
 <a class="theme-link" target="_blank" href="https://hexo.io" rel="external nofollow">
 
 <a class="theme-link" target="_blank" href="https://github.com/iissnan/hexo-theme-next" rel="external nofollow">
 ```

 修改`themes/next/layout/_macro/sidebar.swig`，对部分<a>标签加上 `rel="external nofollow"` ，如下所示
 ``` 
<a href="https://creativecommons.org/{% if theme.creative_commons === 'zero' %}publicdomain/zero/1.0{% else %}licenses/{{ theme.creative_commons }}/4.0{% endif %}/" class="cc-opacity" target="_blank" rel="external nofollow">

<a href="{{ link }}" title="{{ name }}" target="_blank" rel="external nofollow">{{ name }}</a>
 ```



