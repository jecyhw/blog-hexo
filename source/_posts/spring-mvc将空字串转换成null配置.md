---
title: spring mvc将空字串转换成null配置
tags:
  - spring mvc
categories:
  - java
abbrlink: 4075928980
date: 2018-04-05 21:28:23
---

在applicationContext配置文件中添加
<!-- more -->
```
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="customEditors">
        <map>
            <entry key="java.lang.String">
                <bean class="org.springframework.beans.propertyeditors.StringTrimmerEditor">
                    <constructor-arg name="emptyAsNull" value="true"/>
                </bean>
            </entry>
        </map>
    </property>
</bean>
```