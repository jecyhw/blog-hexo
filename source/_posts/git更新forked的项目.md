---
title: git更新forked的项目
tags:
  - git
categories:
  - git
abbrlink: 2518201122
date: 2018-05-05 18:02:47
---

如何更新forked过来的项目。
<!-- more -->
方法如下：
1. 添加原项目的remote源,只需要添加一次就行
```
git remote add upstream git@github.com:soimort/you-get.git
``` 
2. 将本地修改的commit下
3. push前执行
``` 
git remote update upstream
git checkout {branch name}
git rebase upstream/{branch name}
```
4. 最后进行push
``` 
git push origin {branch name}
```