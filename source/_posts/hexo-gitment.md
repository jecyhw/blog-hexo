---
title: hexo和gitment评论
tags:
  - hexo
  - gitment
categories:
  - hexo
abbrlink: 652457385
date: 2018-04-05 18:44:23
---
hexo next主题配置gitment评论
<!-- more -->
### 创建OAuth App

登录github，进入
> https://github.com/settings/developers

点击 New OAuth APP,填写OAuth application内容，如下图所示
![Register a new OAuth application](hexo-gitment\991b635b.png)

填写完后，点击注册，成功之后可以看到Client ID和Client Secret，在后面需要用到。

### 创建repository，用来存储评论
如下图所示，这里创建的repository是hexo-gitment
![Create a new repository](hexo-gitment\a93202a6.png)

### 在hexo next主题中配置
在next的_config.yml文件中找到gitment配置处
![_config.yml](hexo-gitment\36a0a26d.png)
### 修改gitment.swig文件
gitment.swig文件在next/layout/_third-party/comments下
将 `id: window.location.pathname,`修改为 `id: '{{page.date}}',`如下所示
``` 
function renderGitment(){
    var gitment = new {{CommentsClass}}({
        id: '{{page.date}}',
        owner: '{{ theme.gitment.github_user }}',
        repo: '{{ theme.gitment.github_repo }}',
        {% if theme.gitment.mint %}
        lang: "{{ theme.gitment.language }}" || navigator.language || navigator.systemLanguage || navigator.userLanguage,
        {% endif %}
        oauth: {
        {% if theme.gitment.mint and theme.gitment.redirect_protocol %}
            redirect_protocol: '{{ theme.gitment.redirect_protocol }}',
        {% endif %}
        {% if theme.gitment.mint and theme.gitment.proxy_gateway %}
            proxy_gateway: '{{ theme.gitment.proxy_gateway }}',
        {% else %}
            client_secret: '{{ theme.gitment.client_secret }}',
        {% endif %}
            client_id: '{{ theme.gitment.client_id }}'
        }});
    gitment.render('gitment-container');
}
```
### 部署
配置完成后，`hexo g -d`，需要等待一会才会好。

### 初始化评论
每篇文章第一次评论时，都需要点击一次`初始化本文的评论页`
