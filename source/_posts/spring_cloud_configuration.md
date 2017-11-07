---
title: SpringCloud 配置概览
date: 2017/11/7 12:00:00
tags: Spring Cloud
---

Spring Cloud相关组件的依赖注解及配置，自己做个笔记。根据以下博客整理
- http://blog.csdn.net/forezp/article/details/70148833
- http://www.cnblogs.com/skyblog/p/5127690.html


### 基础pom结构

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yourgroup</groupId>
    <artifactId>yourartifact</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>


    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.2.RELEASE</version>
        <relativePath/>
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--your dependencies -->

    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Dalston.RC1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>
</project>
```

### 分布式配置中心-Spring Cloud Config
#### dependence
```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
```
#### 启动类注解

```
@EnableConfigServer
```
#### application.yml

```
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: ...
          searchPaths: ... 
          username: ...
          password: ...
```
### 服务的注册与发现-EurekaServer
#### server端
##### dependence

```
        <!--eureka server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>

        <!--监控模块 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```
##### 启动类注解

```
@EnableEurekaServer
```

##### application.yml

```
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/

```
#### client端
##### dependence

```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

##### application.yml
```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8762

spring:
  application:
    name: client1
```

#####  bootstrap.yml
```
spring:
  cloud:
    config:
      uri: http://localhost1:${config.port:8888}
      name: cloud-config
      profile: ${config.profile:dev}
```
### 断路器-Hystrix
#### dependence

```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
        </dependency>
```
#### 启动类注解

```
@EnableEurekaClient
@EnableHystrix
```


#### application.yml

```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8764

spring:
  application:
    name: web
```
#### controller层

```

@RestController
public class DataController {

    @Autowired
    DataService dataService;

    @RequestMapping(value="/data1")
    public ResponseEntity<String> data1(){

        String data=dataService.getData1();
        return new ResponseEntity<String>(data, HttpStatus.OK);

    }

    @RequestMapping(value="/data2")
    public ResponseEntity<String> data2(){

        String data=dataService.getData2();
        return new ResponseEntity<String>(data, HttpStatus.OK);

    }

}
```
#### service层

```
@Service
public class DataService {

    @Autowired
    RestTemplate restTemplate;

    final String SERVICE_NAME1 = "client1";
    final String SERVICE_NAME2 = "client2";

    @HystrixCommand(fallbackMethod = "fallbackSearchAll")
    public String getData1() {
        return restTemplate.getForObject("http://" + SERVICE_NAME1 + "/client1", String.class);
    }

    @HystrixCommand(fallbackMethod = "fallbackSearchAll")
    public String getData2() {
        return restTemplate.getForObject("http://" + SERVICE_NAME2 + "/client2", String.class);
    }

    private String fallbackSearchAll() {
        System.out.println("HystrixCommand fallbackMethod handle!");
        return "HystrixCommand fallbackMethod handle success!";
    }

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

### 路由网关-zuul
#### dependence

```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zuul</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```
#### 启动类注解

```
@EnableZuulProxy
@EnableEurekaClient
```
#### application.yml

```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8766

spring:
  application:
    name: zuul

zuul:
  routes:
    api-a:
      path: /api-a/**
      serviceId: client1
    api-b:
      path: /api-b/**
      serviceId: client2
```

### 消息总线-Spring Cloud Bus
#### rabbitmq
需要事先安装rabbitmq，并且运行
#### dependence
在Spring Cloud Config的pom文件的基础上新增rabbitmq相关配置

```
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/wharvey/cloud-config-repo.git
          searchPaths: cloud-config-repo
          username: 31831368@qq.com
          password: flzx3qc1


  rabbitmq:
    host: localhost
    port: 15672
    username: guest
    password: guest
```
#### refresh
修改git仓库中的配置文件后，刷新eureka server
http://localhost:8761/bus/refresh
