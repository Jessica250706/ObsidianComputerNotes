---
tags:
  - Android
beginDate: 2025-09-24
---
# 0.推荐技术

MPAndroidChart：安卓图表技术

# 1.技术

## 1.1 跨平台开发技术

Eg. `Flutter`、`React Native`、`Uni-app` 、`Taro`

### 1.1.1 `Flutter`

Flutter 是由 Google 开发的跨平台移动应用开发框架。使用 Dart 语言进行开发。Flutter 具有出色的性能、渲染速度和用户体验，可以实现流畅的动画效果，支持一套代码同时在 `iOS`、`Android`、PC 端上运行。且可以构建适用于任何屏幕的应用程序。

国内首款 Flutter 的大型应用：咸鱼。

### 1.1.2 `React Native`

React Native (简称 RN) 是 Facebook 于 2015 年 4 月开源的跨平台移动应用开发框架，是 Facebook 早先开源的 JS 框架 React 在原生移动应用平台的衍生产物，支持 iOS 和 Android 两大平台。

使用 Javascript 语言，React 和 React Native 都使用 JSX 语法，这种语法使得你可以在 JavaScript 中直接输出元素，例如：`<Text>Hello, I am your cat!</Text>`。

### 1.1.3 `Uni-app`

Uni-app 是一种基于 Vue.js 的**跨平台开发框架**，由 DCloud 公司开发和维护。它允许开发者使用一套代码同时构建多个平台的应用程序，包括 iOS、Android、`H5`、微信小程序、支付宝小程序、百度小程序、字节跳动小程序等。

Uni-app 的核心思想是“写一次，到处运行”。开发者只需编写一次代码，就可以生成在不同平台上运行的应用程序。这样的开发方式极大地提高了开发效率，减少了开发成本。

Uni-app 基于 Vue.js 语法和组件模型进行开发。Vue.js 是一种流行的 JavaScript 框架，广泛应用于前端开发。通过使用 Vue.js 的语法和生态系统中的丰富插件和工具，开发者可以快速构建出功能强大且易于维护的应用程序。

### 1.1.4 `Taro`

Taro 是一套京东打造的遵循 React 语法规范的多端开发解决方案。使用 React、Vue 代码等编程，使用 Taro 时，只书写一套代码，再通过 Taro 的编译工具，将源代码分别编译出可以在不同端（微信小程序、`H5`、Android 和 iOS 原生 app 等）运行的代码。

Eg. 美团外卖小程序、喜马拉雅 FM 小程序、京东小程序

## 1.2 Android

### 1.2.1 官方语言

主要：Java 和 Kotlin

页面布局：XML

数据库：SQLite（内置）

#### 1.2.1.1 数据库 SQLite

- 客户端与服务端分别操作的数据库

![504](images/Pasted%20image%2020250924202845.png)

- 客户端与服务端的多对一架构关系

![563](images/Pasted%20image%2020250924202930.png)

# 2.APP 工程

## 2.1 目录结构

### 2.1.1 层次

1. 项目
2. 模块

> 模块依附于项目，每个项目至少有一个模块，也能拥有多个模块。
> 一般所言的“编译运行 App”，指的是运行某个模块，而非运行某个项目，因为**模块对应实际的 App**。

#### 2.1.1.1 项目

1. app（代表app模块）

```
manifests子目录，存放AndroidManifest.xml，它是App的运行配置文件。
java子目录，存放当前模块的Java源代码。
res子目录，存放当前模块的资源文件。
```

2. Gradle Scripts（主要是工程的编译配置文件）

```
build.gradle，该文件分为 项目级 与 模块级 两种，用于描述App工程的编译规则。
proguard-rules.pro，该文件描述了Java代码的混淆规则。
gradle.properties，该文件配置了编译工程的命令行参数，一般无须改动。
settings.gradle，该文件配置了需要编译哪些模块，以及依赖库的仓库地址。
local.properties，它是项目的本地配置文件。
```

# 3.新建项目

