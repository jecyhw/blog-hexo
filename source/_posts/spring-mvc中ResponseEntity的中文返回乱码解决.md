---
title: spring mvc中ResponseEntity的中文返回乱码解决
tags:
  - spring mvc
categories:
  - java
abbrlink: 126721933
date: 2018-04-05 21:32:06
---
spring mvc中ResponseEntity的中文返回乱码解决方法
<!-- more -->
### 方法1
在`@RequestMapping`注解中声明`prodeces={"application/json; charset=utf-8"}`可以解决

### 方法2

在dispatcher-servlet配置文件中添加以下配置即可
```
<mvc:annotation-driven>
    <mvc:message-converters><!-- 处理请求时返回字符串的中文乱码问题 -->
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="writeAcceptCharset" value="false"/>
            <property name="supportedMediaTypes">
                <list>
                    <value>application/xml;charset=UTF-8</value>
                    <value>text/html;charset=UTF-8</value>
                    <value>text/plain;charset=UTF-8</value>
                    <value>application/json;charset=UTF-8</value>
                </list>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```