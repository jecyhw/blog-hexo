---
title: spring boot - Live Reload/Hot Swap
tags:
  - spring boot``
  - hot swap
categories:
  - java
abbrlink: 3435526137
date: 2018-04-05 21:24:05
---
spring boot开发环境热部署，修改后实时更新
<!-- more -->
### 项目配置依赖
Maven,添加
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<scope>runtime</scope>
</dependency>
```
Gradle，添加
```
runtime('org.springframework.boot:spring-boot-devtools')
```
### IDEA设置
`File -> Settings -> Build, Exception, Deployment -> Compiler`
> Make project automatically勾选上

`Ctrl + Shift + A`,输入`Registry`
> 找到 compiler.automake.allow.when.app.running,勾选上

### 安装chrome插件: livereload