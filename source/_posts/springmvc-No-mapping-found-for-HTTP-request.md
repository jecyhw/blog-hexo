---
title: springmvc No mapping found for HTTP request
tags:
  - springmvc
categories:
  - java
abbrlink: 1542896872
date: 2018-04-08 09:06:02
---

spring mvc 出现No mapping found for HTTP request with URI /css/main.css in DispatcherServlet with name 'springmvc'错误解决
<!-- more -->

### 方法1
 在web.xml文件上配置
 ``` 
 <servlet-mapping>
     <servlet-name>default</servlet-name>
     <url-pattern>*.css</url-pattern>
 </servlet-mapping>
 ```
 ### 方法2
 在spring-mvc配置文件中配置
 ``` 
 # 先在spring-mvc配置文件中声明spring-mvc标签
 <beans xmlns="http://www.springframework.org/schema/beans"
    ...
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="...
    http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
    ">
 # 再添加
 <mvc:default-servlet-handler/>
 # 或者
 <mvc:resources mapping="/**" location="/" />
 ```