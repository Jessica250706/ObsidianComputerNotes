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

