---
tags:
  - 项目
  - SpringBoot
  - VUE3
beginDate: 2026-09-07
endDate:
---
# 0.参考视频

【黑马程序员 Java 项目实战《苍穹外卖》，最适合新手的 SpringBoot+SSM 的企业级 Java 项目实战】https://www.bilibili.com/video/BV1TP411v7v6?vd_source=4e42d3c23020c1c6dc6a9aac2d11ab9c

个人学习仓库：[Jessica250706/hm-takeout: 黑马苍穹外卖](https://github.com/Jessica250706/hm-takeout)

# 1.开发环境搭建

## 1.1 前端

启动 nginx：双击 nginx.exe 即可启动 nginx 服务，访问端口号为 80。

注意，必须要在全英文的目录下启动，否则无法运行。且，nginx.exe 在 `hm-takeout\Documents\front-end-environment\nginx-1.20.2` 中。

## 1.2 后端

### 1.2.1 项目结构

sky-common 子模块中存放的是一些公共类，可以供其他模块使用

sky-pojo 子模块中存放的是一些 entity、DTO、VO

| 名称     | 说明                                  |
| ------ | ----------------------------------- |
| POJO   | 普通 Java 对象，只有属性和对应的 getter 和 setter |
| Entity | 实体，通常和数据库中的表对应                      |
| DTO    | 数据传输对象，通常用于程序中各层之间传递数据              |
| VO     | 视图对象，为前端展示数据提供的对象                   |

sky-server 子模块中存放的是 配置文件、配置类、拦截器、controller、service、mapper、启动类等

### 1.2.2 数据库环境

导入数据库。

### 1.2.3 前后端联调

先确保 `application-dev.yml` 中的数据库的用户名和密码与本地数据库一致。随后启动程序，测试登录功能是否正常。

#### 1.2.3.1 Nginx 反向代理

nginx 反向代理，就是将前端发送的动态请求由 nginx 转发到后端服务器

![image-HM-苍穹外卖（takeout）-nigix反向代理.png](images/image-HM-苍穹外卖（takeout）-nigix反向代理.png)

好处：

- 提高访问速度
- 进行负载均衡
- 保证后端服务安全

P.s. 所谓负载均衡,就是把大量的请求按照我们指定的方式均衡的分配给集群中的每台服务器

### 1.2.4 完善登录功能

1. 修改数据库中的密码。

```text title:'123456的MD5加密后的密文'
e10adc3949ba59abbe56e057f20f883e
```

2. 添加后端中加密密码的逻辑。

```java title:'EmployeeServiceImpl.java'
password = DigestUtils.md5DigestAsHex(password.getBytes());
```

## 1.3 接口文档

使用了 Apifox，集成了 Yapi 和 Swagger 的功能。

通过 Apifox 导入接口文档即可。

# 2.员工

## 2.1 新增员工

### 2.1.1 代码开发

详见代码仓库。

注意 mapper 文件中的注入 SQL 不要输入错误，必须与 Employee 实体类中的字段保持一致。

### 2.1.2 功能测试

如果使用的是 Apifox，先选择环境。

![image-HM-苍穹外卖（takeout）-Apifox配置开发环境1.png](images/image-HM-苍穹外卖（takeout）-Apifox配置开发环境1.png)

点击管理环境，输入模块的前置 URL。（一般默认是 `http://localhost:8080`）

![image-HM-苍穹外卖（takeout）-Apifox配置环境2.png](images/image-HM-苍穹外卖（takeout）-Apifox配置环境2.png)

调试员工登录接口，添加后置操作。

![image-HM-苍穹外卖（takeout）-Apifox添加后置操作获取登录token.png](images/image-HM-苍穹外卖（takeout）-Apifox添加后置操作获取登录token.png)

随后，添加全局参数。注意，在苍穹外卖项目中，参数名是 token，但在一般项目中，参数名是请求头（Authorization）。

![image-HM-苍穹外卖（takeout）-Apifox配置全局参数.png](images/image-HM-苍穹外卖（takeout）-Apifox配置全局参数.png)

然后再测试新增员工接口，若返回数据中 code 为 1，且数据库中出现新数据，则证明测试成功。

## 2.2 员工分页查询

## 2.3 启用禁用员工账号

## 2.4 编辑员工

