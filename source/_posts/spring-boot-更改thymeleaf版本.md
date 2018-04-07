---
title: spring boot更改thymeleaf版本
tags:
  - spring boot
  - thymeleaf
categories:
  - java
abbrlink: 3210521597
date: 2018-04-05 21:26:39
---

Maven配置
<!-- more -->
```
<properties>
    <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
    <thymeleaf-layout-dialect.version>2.0.5</thymeleaf-layout-dialect.version>
</properties>
```
Gradle配置
```
ext['thymeleaf.version'] = '3.0.2.RELEASE'
ext['thymeleaf-layout-dialect.version'] = '2.0.5'
```
---

提示: spring boot所有的依赖项的版本配置在
```
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>1.4.2.RELEASE</version>
	<relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```
用自己的版本号覆盖就行