---
title: spring boot swagger配置
tags:
  - spring boot
  - swagger
categories:
  - java
abbrlink: 1664483852
date: 2018-04-12 17:07:18
---
spring boot整合swagger搭建restful api文档
<!-- more -->
### 创建spring boot项目
使用 `Spring Initializr` 创建 `spring boot` 项目 
### swagger配置
`maven` 配置
``` 
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.7.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.7.0</version>
</dependency>
```

创建 `Swagger` 配置类
```java 
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Autowired
    private TokenProperties properties;

    ApiInfo apiInfo() {
      return new ApiInfoBuilder()
        .title("API Reference")
        .version("1.0.0")
        .build();
    }

    @Bean
    public Docket customImplementation(){
		return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .select().paths(PathSelectors.any())
                .apis(RequestHandlerSelectors.basePackage("controller")) //controller所在包
                .build()
                .pathMapping("/")
      ;
    }
}
```
### swagger注解

> @Api：修饰整个类，描述Controller的作用

> @ApiImplicitParam：一个请求参数

> @ApiImplicitParams：多个请求参数

> @ApiModel：修饰model对象类

> @ApiModelProperty：修饰model对象类的属性

> @ApiOperation：描述一个类的一个方法，或者说一个接口

> @ApiParam：单个参数描述

> @ApiResponse：HTTP响应其中1个描述

> @ApiResponses：HTTP响应整体描述

### 示例
```java 
@ApiModel("用户信息")
public class User {
    @ApiModelProperty(value = "主键", notes = "数据库自动生成")
    private int id;
    @ApiModelProperty(value = "账号", required = true, notes = "唯一")
    private String account;
    @ApiModelProperty(value = "密码", required = true, hidden = true)
    private String password;
}
```

```java
 @Api(value = "用户API", description = "用户API", tags = "User")
 @RestController
 @RequestMapping("user")
 public class UserController {
 
     @ApiOperation(value = "根据用户id获取用户信息")
      @ApiImplicitParams(
              @ApiImplicitParam(name = "userId", value = "用户id")
      )
     @RequestMapping(value = "get", method = RequestMethod.GET)
     public HttpRes<User> getAll(@RequestParam int userId) {
         // ...
     }
 }

 ```
 启动 `spring boot`，访问 `http://localhost:8080/swagger-ui.html` 就可以看到效果了。
 ![示例](spring-boot-swagger配置\408ce14c.png)