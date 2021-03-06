---
title: 常用maven jar包
date: 2017/6/28
tags: maven
---

日志

```
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-api</artifactId>
	<version>1.7.25</version>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.7.25</version>
	<scope>test</scope>
</dependency>
```
office工具包
```
<dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi</artifactId>
	<version>3.16</version>
</dependency>
<dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi-ooxml</artifactId>
	<version>3.16</version>
</dependency>
```

<!-- more -->

fastjson

```
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>fastjson</artifactId>
	<version>1.2.24</version>
</dependency>
```
json

```
<dependency>
	<groupId>net.sf.json-lib</groupId>
	<artifactId>json-lib</artifactId>
	<version>2.4</version>
	<classifier>jdk15</classifier>
</dependency>
```
mysql
```
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>5.1.39</version>
</dependency>
```
c3p0

```
<dependency>
	<groupId>c3p0</groupId>
	<artifactId>c3p0</artifactId>
	<version>0.9.1.2</version>
</dependency>
```
jFinal

```
<dependency>
	<groupId>com.jfinal</groupId>
	<artifactId>jfinal</artifactId>
	<version>3.0</version>
</dependency>
```
servlet,jsp相关

```
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>servlet-api</artifactId>
	<version>2.4</version>
</dependency>
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>jsp-api</artifactId>
	<version>2.0</version>
</dependency>
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>jstl</artifactId>
	<version>1.2</version>
</dependency>
<dependency>
	<groupId>taglibs</groupId>
	<artifactId>standard</artifactId>
	<version>1.1.2</version>
</dependency>
```
webmagic爬虫框架

```
<dependency>
	<groupId>us.codecraft</groupId>
	<artifactId>webmagic-core</artifactId>
	<version>0.5.3</version>
</dependency>
<dependency>
	<groupId>us.codecraft</groupId>
	<artifactId>webmagic-extension</artifactId>
	<version>0.5.3</version>
</dependency>
```
selenium-java

```
<dependency>
	<groupId>org.seleniumhq.selenium</groupId>
	<artifactId>selenium-java</artifactId>
	<version>2.53.0</version>
</dependency>
```
