---
title: Swagger2在Spring Boot项目中的简单使用
date: 2017/11/8 18:00:00
tags: [Spring Boot, Swagger]
---

目前大部分Web项目都采用了Restful的方式进行数据交互，如何有效管理众多APIs是我们必须面对的问题。在一阵搜索过后发现了Swagger这个框架，可以很方便地根据代码里的注解生成API文档，而且利用自带的swagger-ui工具，可以在线调试，功能强大。下面简单介绍一下Swagger和Spring Boot项目的集成。

### POM依赖

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- swagger -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.2.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.2.2</version>
        </dependency>
```

<!-- more -->
### Swagger配置类
在Spring Boot启动类的同一目录下新建Swagger配置类

```
@Configuration
@EnableSwagger2
public class Swagger2 {

    //指定包及扫描的路径，下面any()是指定所有，也可以自定义路径
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.harvey.swagger"))
                .paths(PathSelectors.any())
                .build();
    }

    //一些相关信息，会展示在swagger-ui首页
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot中使用Swagger2构建RESTful APIs")
                .description("This is a demo!")
                .contact("harvey")
                .version("1.0")
                .build();
    }

```
### 添加注解

具体注解可参考  
https://gumutianqi1.gitbooks.io/specification-doc/content/tools-doc/spring-boot-swagger2-guide.html

```
@RestController
@RequestMapping("/user")
public class UserController {

    @ApiOperation(value="获取用户", notes="根据用户id获取用户")
    @RequestMapping(value="/{id}", method = RequestMethod.GET)
    public ResponseEntity<User> getUser(@PathVariable String id){

        User user = new User();
        user.setName("Peter");
        user.setAge(22);
        user.setSex("m");
        return new ResponseEntity<User>(user, HttpStatus.OK);

    }

    @ApiOperation(value="新增用户", notes="新增用户")
    @RequestMapping(value="/", method = RequestMethod.POST)
    @ApiImplicitParam(name = "user", value = "用户详细实体user", required = true, dataType = "User")
    public ResponseEntity<User> addUser(@RequestBody User user){

        System.out.println(user.toString());
        return new ResponseEntity<User>(user, HttpStatus.OK);

    }
}

```
### swagger-ui
访问 http://localhost:{port}/swagger-ui.html/ 可以看到API的详细信息，并且可以在线调试。
