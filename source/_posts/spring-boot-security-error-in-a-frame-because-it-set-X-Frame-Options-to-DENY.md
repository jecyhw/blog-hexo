---
title: >-
  spring boot security error: in a frame because it set 'X-Frame-Options' to
  DENY
tags:
  - spring boot
categories:
  - java
abbrlink: 2914312071
date: 2018-04-05 21:21:44
---
spring boot security出现in a frame because it set 'X-Frame-Options' to DENY错误解决办法
<!-- more -->
``` 
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                    .headers() //加上
                    .frameOptions().sameOrigin()
    }
}
```