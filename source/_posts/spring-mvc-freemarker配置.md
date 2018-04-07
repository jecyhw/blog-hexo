---
title: spring mvc freemarker配置
tags:
  - spring mvc
  - freemarker
categories:
  - java
abbrlink: 3913767996
date: 2018-04-05 21:29:49
---
在applicationContext.xml配置文件中配置
<!-- more -->
### 非web的freemarker配置
```
<bean id="freemarkerConfiguration" class="org.springframework.ui.freemarker.FreeMarkerConfigurationFactoryBean">
    <!--模板所在目录-->
    <property name="templateLoaderPath" value="classpath:template" /> 
    <!--freemarker配置文件-->
    <property name="configLocation" value="classpath:freemarker.properties"/>
    <property name="freemarkerVariables">
        <map>
            <entry key="tileSize" value="${tileSize}"/>
            <entry key="tileFormat" value="${tileFormat}"/>
            <entry key="maxLevel" value="${maxLevel}"/>
        </map>
    </property>
</bean>
```
### web的freemarker配置
```
<bean class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
    <!--模板所在目录-->
    <property name="templateLoaderPath" value="classpath:template" />
    <!--freemarker配置文件-->
    <property name="configLocation" value="classpath:freemarker.properties"/>
    <property name="freemarkerVariables">
        <map>
            <entry key="tileSize" value="${tileSize}"/>
            <entry key="tileFormat" value="${tileFormat}"/>
            <entry key="maxLevel" value="${maxLevel}"/>
        </map>
    </property>
</bean>
```

在freemarker的`Configuration`类实例注入该bean即可
两者的区别在于`FreeMarkerConfigurer`依赖与`ServletContext`