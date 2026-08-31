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
        userMapper.add(username, md5String);  
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

Step 2）在参数前面添加@Pattern 注解

Step 3）在 Controller 类上添加@Validated 注解

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

### 2.1.6 参数校验失败异常处理

新建 exception/GlobalExceptionHandler.java 文件。（全局异常处理器）

```java title:'com/xq/exception/GlobalExceptionHandler.java' hl:4
package com.xq.exception;  
  
import com.xq.pojo.Result;  
import org.springframework.util.StringUtils;  
import org.springframework.web.bind.annotation.ExceptionHandler;  
import org.springframework.web.bind.annotation.RestControllerAdvice;  
  
@RestControllerAdvice  
public class GlobalExceptionHandler {  
  
    @ExceptionHandler(Exception.class)  
    public Result handleException(Exception e) {  
        e.printStackTrace();  
        return Result.error(StringUtils.hasLength(e.getMessage()) ? e.getMessage() : "操作失败");  
    }  
}
```

注意，`StringUtils` 引入的是 `org.springframework.util` 包中的。

## 2.2 登录

### 2.2.1 登录主逻辑

新增登录接口。

```java title:'com/xq/service/impl/UserServiceImpl.java'
@PostMapping("/login")  
public Result<String> login(@Pattern(regexp = "^\\S{5,16}$") String username, @Pattern(regexp = "^\\S{5,16}$") String password) {  
    // 根据用户名查询用户  
    User loginUser = userService.findByUserName(username);  
    // 判断该用户是否存在  
    if (loginUser == null) {  
        return Result.error("用户名错误！");  
    }  
    // 判断密码是否正确（loginUser对象中的password是密文）  
    if (Md5Util.getMD5String(password).equals(loginUser.getPassword())) {  
        // 登陆成功  
        return Result.success("jwt token令牌…");  
    }  
    return Result.error("密码错误！");  
}
```

### 2.2.2 JWT 令牌

简介。

![image-HM-大事件（big-event）-JWT简介.png](images/image-HM-大事件（big-event）-JWT简介.png)

引入依赖，并刷新 Maven。

```xml title:'pom.xml'
<!--  JWT依赖  -->  
<dependency>  
  <groupId>com.auth0</groupId>  
  <artifactId>java-jwt</artifactId>  
  <version>4.4.0</version>  
</dependency>  
<!--  单元测试的坐标  -->  
<dependency>  
  <groupId>org.springframework.boot</groupId>  
  <artifactId>spring-boot-starter-test</artifactId>  
</dependency>
```

新增 `JwtTest` 文件，进行单元测试。若 JWT 无对应导入包，则“文件->清除缓存->勾选清除文件系统缓存和本地历史记录->失效并重启”。

```java title:'big-event-back-end/src/test/java/com/xq/JwtTest.java'
package com.xq;  
  
import com.auth0.jwt.JWT;  
import com.auth0.jwt.JWTVerifier;  
import com.auth0.jwt.algorithms.Algorithm;  
import com.auth0.jwt.interfaces.Claim;  
import com.auth0.jwt.interfaces.DecodedJWT;  
import org.junit.jupiter.api.Test;  
  
import java.util.Date;  
import java.util.HashMap;  
import java.util.Map;  
  
public class JwtTest {  
  
    @Test  
    public  void testGen() {  
        Map<String, Object> claims = new HashMap<>();  
        claims.put("id", 1);  
        claims.put("username", "test");  
        // 生成JWT的代码  
        String token = JWT.create()  
                .withClaim("user", claims) // 添加载荷  
                .withExpiresAt(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 24)) // 添加过期时间（24H）  
                .sign(Algorithm.HMAC256("itheima")); // 指定算法，配置秘钥  
  
        System.out.println(token);  
    }  
  
    @Test  
    public void testParse() {  
        // 定义字符串，模拟用户传递过来的token  
        String token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" +  
                ".eyJ1c2VyIjp7ImlkIjoxLCJ1c2VybmFtZSI6InRlc3QifSwiZXhwIjoxNzg4MTIyMTYyfQ" +  
                ".X2TfThPCLbYgUXzrhuvjqlAmWv9RxgMjfctgDe8kFfU";  
  
        JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("itheima")).build();  
  
        DecodedJWT decodedJWT = jwtVerifier.verify(token); // 验证token，生成一个解析后的JWT对象  
        Map<String, Claim> claims = decodedJWT.getClaims();  
        System.out.println(claims.get("user"));  
    }  
  
}
```

### 2.2.3 登录认证

引入资料中的 `JwtUtil.java` 到文件夹 `utils` 中。

修改登录逻辑。

```java title:'UserController.java（节选）'
@PostMapping("/login")  
public Result<String> login(@Pattern(regexp = "^\\S{5,16}$") String username, @Pattern(regexp = "^\\S{5,16}$") String password) {  
    // 根据用户名查询用户  
    User loginUser = userService.findByUserName(username);  
    // 判断该用户是否存在  
    if (loginUser == null) {  
        return Result.error("用户名错误！");  
    }  
    // 判断密码是否正确（loginUser对象中的password是密文）  
    if (Md5Util.getMD5String(password).equals(loginUser.getPassword())) {  
        // 登陆成功  
        Map<String, Object> claims = new HashMap<>();  
        claims.put("id", loginUser.getId());  
        claims.put("username", loginUser.getUsername());  
        String token = JwtUtil.genToken(claims);  
        return Result.success(token);  
    }  
    return Result.error("密码错误！");  
}
```

创建 `interceptors/LoginInterceptor.java` 拦截器文件。

```java title:'com/xq/interceptors/LoginInterceptor.java'
package com.xq.interceptors;  
  
import com.xq.utils.JwtUtil;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.springframework.stereotype.Component;  
import org.springframework.web.servlet.HandlerInterceptor;  
  
import java.util.Map;  
  
@Component  
public class LoginInterceptor implements HandlerInterceptor {  
  
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
        // 令牌验证  
        String token = request.getHeader("Authorization");  
        try {  
            Map<String, Object> claims = JwtUtil.parseToken(token);  
            return true; // 放行  
        } catch (Exception e) {  
            // http响应状态码为401  
            response.setStatus(401);  
            return false; // 拦截  
        }  
    }  
}
```

新建 `config/WebConfig.java` 文件

```java title:'com/xq/config/WebConfig.java'
package com.xq.config;  
  
import com.xq.interceptors.LoginInterceptor;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;  
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;  
  
@Configuration  
public class WebConfig implements WebMvcConfigurer {  
  
    @Autowired  
    private LoginInterceptor loginInterceptor;  
  
    @Override  
    public void addInterceptors(InterceptorRegistry registry) {  
        // 登录和注册接口不拦截  
        registry.addInterceptor(loginInterceptor).excludePathPatterns("/user/login", "/user/register");  
    }  
}
```

## 2.3 获取用户详细信息

### 2.3.1 功能实现

添加接口。

```java title:'com/xq/controller/UserController.java'
@GetMapping("/userInfo")  
public Result<User> userInfo(@RequestHeader(name = "Authorization") String token) {  
    // 根据用户名查询用户  
    Map<String, Object> map = JwtUtil.parseToken(token);  
    String username = (String) map.get("username");  
  
    User user = userService.findByUserName(username);  
    return Result.success(user);  
}
```

注释 password，使返回参数中没有 password。（注意，`@JsonIgnore` 有两个对应包，请正确引入，否则功能不能正常实现。）

```java title:'com/xq/pojo/User.java' hl:3,14
package com.xq.pojo;  
  
import com.fasterxml.jackson.annotation.JsonIgnore;  
import lombok.Data;  
  
import java.time.LocalDateTime;  
  
// lombok 在编译阶段，为实体类自动生成setter、getter、toString  
// 1）pom文件中引入依赖；2）在实体类上添加注解；  
@Data  
public class User {  
    private Integer id;//主键ID  
    private String username;//用户名  
    @JsonIgnore // 让 SpringMVC 把当前对象转换成 json 字符串的时候，忽略 password，最终的 json 字符串中就没有 password 这个属性了  
    private String password;//密码  
    private String nickname;//昵称  
    private String email;//邮箱  
    private String userPic;//用户头像地址  
    private LocalDateTime createTime;//创建时间  
    private LocalDateTime updateTime;//更新时间  
}
```

开启驼峰命名和下划线命名的自动转换。

```yml title:'application.yml'
mybatis:  
  configuration:  
    map-underscore-to-camel-case: true # 开启驼峰命名和下划线命名的自动转换
```

### 2.3.2 `ThreadLocal` 优化

`ThreadLocal` 提供线程局部变量。以下代码用于测试 `ThreadLocal` 的作用。

```java title:'com/xq/ThreadLocalTest.java' group:ThreadLocalTest
package com.xq;  
  
import org.junit.jupiter.api.Test;  
  
public class ThreadLocalTest {  
  
    @Test  
    public void testThreadLocalSetAndGet() {  
        // 提供一个 ThreadLocal 对象  
        ThreadLocal tl = new ThreadLocal();  
        // 开启两个线程  
        new Thread(() -> {  
            tl.set("Yuki");  
            System.out.println(Thread.currentThread().getName() + "：" + tl.get());  
            System.out.println(Thread.currentThread().getName() + "：" + tl.get());  
            System.out.println(Thread.currentThread().getName() + "：" + tl.get());  
        }, "绿色").start();  
        new Thread(() -> {  
            tl.set("Momo");  
            System.out.println(Thread.currentThread().getName() + "：" + tl.get());  
            System.out.println(Thread.currentThread().getName() + "：" + tl.get());  
            System.out.println(Thread.currentThread().getName() + "：" + tl.get());  
        }, "粉色").start();  
    }  
}
```
```text title:'输出' group:ThreadLocalTest
绿色：Yuki
粉色：Momo
绿色：Yuki
粉色：Momo
粉色：Momo
绿色：Yuki
```

从资料中 copy `ThreadLocalUtil` 工具类放入项目的工具类文件夹中。

在登录拦截器中添加代码。

```java title:'com/xq/interceptors/LoginInterceptor.java' hl:20-21,30-34
package com.xq.interceptors;  
  
import com.xq.utils.JwtUtil;  
import com.xq.utils.ThreadLocalUtil;  
import jakarta.servlet.http.HttpServletRequest;  
import jakarta.servlet.http.HttpServletResponse;  
import org.springframework.stereotype.Component;  
import org.springframework.web.servlet.HandlerInterceptor;  
  
import java.util.Map;  
  
@Component  
public class LoginInterceptor implements HandlerInterceptor {  
  
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
        // 令牌验证  
        String token = request.getHeader("Authorization");  
        try {  
            Map<String, Object> claims = JwtUtil.parseToken(token);  
            // 把业务数据存储到 ThreadLocal 中  
            ThreadLocalUtil.set(claims);  
            return true; // 放行  
        } catch (Exception e) {  
            // http响应状态码为401  
            response.setStatus(401);  
            return false; // 拦截  
        }  
    }  
  
    @Override  
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {  
        // 清空 ThreadLocal 中的数据，防止内存泄漏  
        ThreadLocalUtil.remove();  
    }  
}
```

修改接口中的逻辑。

```java title:'com/xq/controller/UserController.java' hl:4
@GetMapping("/userInfo")  
public Result<User> userInfo(@RequestHeader(name = "Authorization") String token) {  
    // 根据用户名查询用户  
    Map<String, Object> map = ThreadLocalUtil.get();  
    String username = (String) map.get("username");  
  
    User user = userService.findByUserName(username);  
    return Result.success(user);  
}
```

## 2.4 更新用户基本信息

### 2.4.1 功能实现

```java title:'UserController.java'
@PutMapping("/update")  
public Result update(@RequestBody User user) {  
    userService.update(user);  
    return Result.success();  
}
```

```java title:'UserService.java'
// 更新  
void update(User user);
```

```java title:'UserServiceImpl.java'
@Override  
public void update(User user) {  
    user.setUpdateTime(LocalDateTime.now());  
    userMapper.update(user);  
}
```

```java title:'UserMapper.java'
// 更新  
@Update("update user set nickname=#{nickname}, email=#{email}, update_time=#{updateTime} where id=#{id}")  
void update(User user);
```

添加对应代码后，启动程序，使用 Postman / Apifox 进行测试。发送请求后，若数据库数据更新，则说明功能已实现。

### 2.4.2 参数校验

| 注解       | 作用               |
| -------- | ---------------- |
| NotNull  | 值不能为null         |
| NotEmpty | 值不能为null,并且内容不为空 |
| Email    | 满足邮箱格式           |

在实体类中的参数前添加注释。

```java title:'User.java' hl:19,36-37,43-44
package com.xq.pojo;

import com.fasterxml.jackson.annotation.JsonIgnore;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotEmpty;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Pattern;
import lombok.Data;

import java.time.LocalDateTime;

// lombok 在编译阶段，为实体类自动生成setter、getter、toString
// 1）pom文件中引入依赖；2）在实体类上添加注解；
@Data
public class User {
    /*
    * 主键ID
    * */
    @NotNull
    private Integer id;

    /*
    * 用户名
    * */
    private String username;

    /*
    * 密码
    * */
    @JsonIgnore // 让 SpringMVC 把当前对象转换成 json 字符串的时候，忽略 password，最终的 json 字符串中就没有 password 这个属性了
    private String password;

    /*
    * 昵称
    * */
    @NotEmpty
    @Pattern(regexp = "^\\S{1,10}$")
    private String nickname;

    /*
    * 邮箱
    * */
    @NotEmpty
    @Email
    private String email;

    /*
    * 用户头像地址
    * */
    private String userPic;

    /*
    * 创建时间
    * */
    private LocalDateTime createTime;

    /*
    * 更新时间
    * */
    private LocalDateTime updateTime;
}
```

在接口的参数前添加 `@Validated` 注释。

```java title:'UserController.java' hl:2
@PutMapping("/update")  
public Result update(@RequestBody @Validated User user) {  
    userService.update(user);  
    return Result.success();  
}
```

## 2.5 更新用户头像

新增接口，并且在参数 `avatarUrl` 前添加 `@URL` 注解，确保传入的参数是一个 URL。

```java title:'UserController.java'
@PatchMapping("updateAvatar")  
public Result updateAvatar(@RequestParam @URL String avatarUrl) {  
    userService.updateAvatar(avatarUrl);  
    return Result.success();  
}
```

```java title:'UserService.java'
// 更新头像  
void updateAvatar(String avatarUrl);
```

```java title:'UserServiceImpl.java'
@Override  
public void updateAvatar(String avatarUrl) {  
    Map<String, Object> map = ThreadLocalUtil.get();  
    Integer id = (Integer) map.get("id");  
    userMapper.updateAvatar(avatarUrl, id);  
}
```

```java title:'UserMapper.java'
// 更新头像  
@Update("update user set user_pic=#{avatarUrl}, update_time=now() where id=#{id}")  
void updateAvatar(String avatarUrl, Integer id);
```

随后启动程序，并进行测试。

## 2.6 更新用户密码

```java title:'UserController.java'
@PatchMapping("/updatePwd")  
public Result updatePwd(@RequestBody Map<String, String> params) {  
    // 1.校验参数  
    String oldPwd = params.get("old_pwd");  
    String newPwd = params.get("new_pwd");  
    String rePwd = params.get("re_pwd");  
  
    // 1.1 检验是否为空  
    if (!StringUtils.hasLength(oldPwd) || !StringUtils.hasLength(newPwd) || !StringUtils.hasLength(rePwd)) {  
        return Result.error("缺少必要的参数！");  
    }  
  
    // 1.2 原密码是否正确  
    // 调用 userService 根据用户名拿到原密码，再和 oldPwd 比对  
    Map<String, Object> map = ThreadLocalUtil.get();  
    String username = (String) map.get("username");  
    User loginUser = userService.findByUserName(username);  
    if (!loginUser.getPassword().equals(Md5Util.getMD5String(oldPwd))) {  
        return Result.error("原密码填写不正确！");  
    }  
  
    // 1.3 newPwd 与 rePwd 是否一致  
    if (!rePwd.equals(newPwd)) {  
        return Result.error("两次填写的新密码不一样！");  
    }  
  
    // 2.调用 service 完成密码更新  
    userService.updatePwd(newPwd);  
  
    return Result.success();  
}
```

```java title:'UserService.java'
// 更新密码  
void updatePwd(String newPwd);
```

```java title:'UserServiceImpl.java'
@Override  
public void updatePwd(String newPwd) {  
    Map<String, Object> map = ThreadLocalUtil.get();  
    Integer id = (Integer) map.get("id");  
    userMapper.updatePwd(Md5Util.getMD5String(newPwd), id);  
}
```

```java title:'UserMapper.java'
// 更新密码  
@Update("update user set password=#{md5String}, update_time=now() where id=#{id}")  
void updatePwd(String md5String, Integer id);
```

# 3.文章分类

## 3.1 新增文章分类



## 3.2 文章分类列表

## 3.3 获取文章分类详情

## 3.4 更新文章分类

## 3.5 删除文章分类

# 4.文章管理

## 4.1 新增文章

## 4.2 文章列表（条件分页）

## 4.3 获取文章详情

## 4.4 更新文章

## 4.5 删除文字

# 5.文件上传

# 6.登录优化-redis

## 6.1 SpringBoot 继承 redis

## 6.2 令牌主动失效

# 7.SpringBoot 项目部署

# 8.属性配置方式

# 9.多环境开发-Profiles

# 10.新特性-原生镜像

