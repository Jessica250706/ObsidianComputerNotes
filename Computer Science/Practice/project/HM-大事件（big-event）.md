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

### 2.1.1 引入 `lombok` 依赖

```xml title:'pom.xml'
<!--  lombok依赖  -->  
<dependency>  
  <groupId>org.projectlombok</groupId>  
  <artifactId>lombok</artifactId>  
</dependency>
```

引入 `lombok` 依赖后，在 `User` 实体类中添加注释，接着点击 Maven 中的 compile 重新编译。

![image-HM-大事件（big-event）-引入lombok依赖并重新编译.png](images/image-HM-大事件（big-event）-引入lombok依赖并重新编译.png)

然后可以看到对应 target 中已有 getter 等。

![image-HM-大事件（big-event）-1788064331709.png](images/image-HM-大事件（big-event）-1788064331709.png)

如法炮制，在其他实体类中同样添加 `@Data` 注释。

### 2.1.2 导入 Result 实体类

从资料中复制 Result 实体类到代码中，并加上注释。

```java title:'com/xq/pojo/Result.java'
package com.xq.pojo;  
  
import lombok.AllArgsConstructor;  
import lombok.NoArgsConstructor;  
  
//统一响应结果  
@NoArgsConstructor
@AllArgsConstructor  
@Data
public class Result<T> {  
    private Integer code;//业务状态码  0-成功  1-失败  
    private String message;//提示信息  
    private T data;//响应数据  
  
    //快速返回操作成功响应结果(带响应数据)  
    public static <E> Result<E> success(E data) {  
        return new Result<>(0, "操作成功", data);  
    }  
  
    //快速返回操作成功响应结果  
    public static Result success() {  
        return new Result(0, "操作成功", null);  
    }  
  
    public static Result error(String message) {  
        return new Result(1, message, null);  
    }  
}
```

### 2.1.3 实现注册逻辑

添加 `Md5` 加密密码工具类。

![image-HM-大事件（big-event）-Md5工具类.png](images/image-HM-大事件（big-event）-Md5工具类.png)

```java title:'com/xq/utils/Md5Util.java'
package com.xq.utils;

import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class Md5Util {
    /**
     * 默认的密码字符串组合，用来将字节转换成 16 进制表示的字符,apache校验下载的文件的正确性用的就是默认的这个组合
     */
    protected static char hexDigits[] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};

    protected static MessageDigest messagedigest = null;

    static {
        try {
            messagedigest = MessageDigest.getInstance("MD5");
        } catch (NoSuchAlgorithmException nsaex) {
            System.err.println(Md5Util.class.getName() + "初始化失败，MessageDigest不支持MD5Util。");
            nsaex.printStackTrace();
        }
    }

    /**
     * 生成字符串的md5校验值
     *
     * @param s
     * @return
     */
    public static String getMD5String(String s) {
        return getMD5String(s.getBytes());
    }

    /**
     * 判断字符串的md5校验码是否与一个已知的md5码相匹配
     *
     * @param password  要校验的字符串
     * @param md5PwdStr 已知的md5校验码
     * @return
     */
    public static boolean checkPassword(String password, String md5PwdStr) {
        String s = getMD5String(password);
        return s.equals(md5PwdStr);
    }


    public static String getMD5String(byte[] bytes) {
        messagedigest.update(bytes);
        return bufferToHex(messagedigest.digest());
    }

    private static String bufferToHex(byte bytes[]) {
        return bufferToHex(bytes, 0, bytes.length);
    }

    private static String bufferToHex(byte bytes[], int m, int n) {
        StringBuffer stringbuffer = new StringBuffer(2 * n);
        int k = m + n;
        for (int l = m; l < k; l++) {
            appendHexPair(bytes[l], stringbuffer);
        }
        return stringbuffer.toString();
    }

    private static void appendHexPair(byte bt, StringBuffer stringbuffer) {
        char c0 = hexDigits[(bt & 0xf0) >> 4];// 取字节中高 4 位的数字转换, >>>
        // 为逻辑右移，将符号位一起右移,此处未发现两种符号有何不同
        char c1 = hexDigits[bt & 0xf];// 取字节中低 4 位的数字转换
        stringbuffer.append(c0);
        stringbuffer.append(c1);
    }

}
```

依次新建功能所需文件。

![image-HM-大事件（big-event）-1788066252055.png|324](images/image-HM-大事件（big-event）-1788066252055.png)

以下是代码。

```java title:'com/xq/controller/UserController.java'
package com.xq.controller;

import com.xq.pojo.Result;
import com.xq.pojo.User;
import com.xq.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping("/register")
    public Result register(String username, String password) {
        // 查询用户（是否占用）
        User user = userService.findByUserName(username);
        if (user == null) {
            // 没有占用，进行注册
            userService.register(username, password);
            return Result.success();
        } else {
            // 占用
            return Result.error("用户名已被占用");
        }
    }
}
```

```java title:'com/xq/service/UserService.java'
package com.xq.service;  
  
import com.xq.pojo.User;  
  
public interface UserService {  
    // 根据用户名查询用户  
    User findByUserName(String username);  
  
    // 注册  
    void register(String username, String password);  
}
```

```java title:'com/xq/service/impl/UserServiceImpl.java'
package com.xq.service.impl;  
  
import com.xq.mapper.UserMapper;  
import com.xq.pojo.User;  
import com.xq.service.UserService;  
import com.xq.utils.Md5Util;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Service;  
  
@Service  
public class UserServiceImpl implements UserService {  
  
    @Autowired  
    private UserMapper userMapper;  
  
    @Override  
    public User findByUserName(String username) {  
        User user = userMapper.findByUserName(username);  
        return user;  
    }  
  
    @Override  
    public void register(String username, String password) {  
        // 加密密码  
        String md5String = Md5Util.getMD5String(password);  
        // 注册  
        userMapper.add(username, password);  
    }  
}
```

参数 `create_time` 和 `update_time` 可以添加注释，使其在需要生成时自动添加。此处为手动添加。

```java title:'com/xq/mapper/UserMapper.java'
package com.xq.mapper;  
  
import com.xq.pojo.User;  
import org.apache.ibatis.annotations.Insert;  
import org.apache.ibatis.annotations.Mapper;  
import org.apache.ibatis.annotations.Select;  
  
@Mapper  
public interface UserMapper {  
    // 根据用户名查询用户  
    @Select("select * from user where username=#{username}")  
    User findByUserName(String username);  
  
    // 注册新用户  
    @Insert("insert into user(username, password, create_time, update_time)" +  
            " values(#{username}, #{password}, now(), now())")  
    void add(String username, String password);  
}
```

### 2.1.4 测试（后端自测）

老师选择的是 postman，因博主此前已下载过 Apifox，所以选择用 Apifox 进行测试。

这里不做详细说明。

### 2.1.5 使用 Spring Validation 对注册接口的参数进行合法性校验

Step 1）引入 Spring Validation 起步依赖

```xml title:'pom.xml'
<!--  Validation依赖  -->  
<dependency>  
  <groupId>org.springframework.boot</groupId>  
  <artifactId>spring-boot-starter-validation</artifactId>  
</dependency>
```

Step 2）在参数前面添加@Pattern注解

Step 3）在Controller类上添加@Validated注解

```java title:'com/xq/controller/UserController.java' hl:15,22
package com.xq.controller;  
  
import com.xq.pojo.Result;  
import com.xq.pojo.User;  
import com.xq.service.UserService;  
import jakarta.validation.constraints.Pattern;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.validation.annotation.Validated;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
@RequestMapping("/user")  
@Validated  
public class UserController {  
  
    @Autowired  
    private UserService userService;  
  
    @PostMapping("/register")  
    public Result register(@Pattern(regexp = "^\\S{5,16}$") String username, @Pattern(regexp = "^\\S{5,16}$") String password) {  
        // 查询用户（是否占用）  
        User user = userService.findByUserName(username);  
        if (user == null) {  
            // 没有占用，进行注册  
            userService.register(username, password);  
            return Result.success();  
        } else {  
            // 占用  
            return Result.error("用户名已被占用");  
        }  
    }  
}
```

### 2.1.6 

## 2.2 登录

## 2.3 获取用户详细信息

## 2.4 更新用户基本信息

## 2.5 更新用户头像

## 2.6 更新用户密码