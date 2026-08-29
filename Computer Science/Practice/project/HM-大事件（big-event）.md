---
tags:
  - 项目
  - VUE3
  - SpringBoot
beginDate: 2026-08-29
endDate:
---
# 0.参考视频

【黑马程序员 SpringBoot 3+Vue 3 全套视频教程，springboot+vue 企业级全栈开发从基础、实战到面试一套通关】https://www.bilibili.com/video/BV14z4y1N7pg?p=14&vd_source=4e42d3c23020c1c6dc6a9aac2d11ab9c

模版网站：https://fe-bigevent-web.itheima.net/login

# 1.环境搭建

- 执行资料中的 big_event.sql 脚本，准备数据库表
- 创建 springboot 工程，引入对应的依赖（web、mybatis、mysql 驱动）
- 配置文件 application.yml 中引入 mybatis 的配置信息
- 创建包结构，并准备实体类

![Pasted image 20260829111914](images/Pasted%20image%2020260829111914.png)

## 1.1 创建数据库

在 Navicat 软件中导入 `big_event.sql` 文件。（无需新建数据库，sql 语句里已有创建语句）

## 1.2 创建 SpringBoot 工程

![Pasted image 20260829114831](images/Pasted%20image%2020260829114831.png)

因缺少结构，故右击 main 文件，选择新建目录，后点击提示，回车，创建成功。

![Pasted image 20260829115051](images/Pasted%20image%2020260829115051.png)

接着在 `resource` 目录下继续创建文件 `application.yml`

![406](images/Pasted%20image%2020260829115406.png)

引入对应依赖。

![Pasted image 20260829115613](images/Pasted%20image%2020260829115613.png)

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.1.3</version>
</parent>
```

删除原有依赖，添加三个新依赖，随后点击刷新 Maven 的按钮。

![Pasted image 20260829122043](images/Pasted%20image%2020260829122043.png)

```xml
<dependencies>
  <!--  web依赖  -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <!--  mybatis依赖  -->
  <dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>3.0.4</version>
  </dependency>
  <!--  MySQL驱动依赖  -->
  <dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
  </dependency>
</dependencies>
```

## 1.3 引入 `myBatis` 的配置信息

![Pasted image 20260829123154](images/Pasted%20image%2020260829123154.png)

```yml
spring:  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: jdbc:mysql://localhost:3306/big_event  
    username: root  
    password: 123456
```

## 1.4 创建包结构与实体类

实体类的代码在资料文件夹中，对应数据库中的三张表。

![Pasted image 20260829123832](images/Pasted%20image%2020260829123832.png)

```
目录结构:
com.xq
	controller
	mapper
	pojo
		Artivle.java
		Category.java
		User.java
	service
		impl
	utils
```

重命名 APP 启动类，并添加代码。

![Pasted image 20260829124236](images/Pasted%20image%2020260829124236.png)

```java file:BigEventApplication.java
package com.xq;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * Hello world!
 *
 */
@SpringBootApplication
public class BigEventApplication
{
    public static void main( String[] args )
    {
        SpringApplication.run(BigEventApplication.class, args);
    }
}
```

运行 SpringBoot，若成功，则证明上述步骤成功。

# 2.用户

## 2.1 注册

