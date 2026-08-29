---
tags:
  - 前端
  - 后端
beginDate: 2025-10-14
---
# 1.网页

## 1.1.小组

成员信息表格：[[https://docs.qq.com/sheet/DTXBxeGZmRUdYbkRP?rtkey=d8cb277296f102c6c1404605eERAv1&qqInfo=eyJtc2dJZCI6Ijc1NjAzMDg0NjQ1NTEyMTUwOTEiLCJjaGF0VHlwZSI6MiwicGVlclVpZCI6IjEwNjIxODc2MjMiLCJlbGVtSWQiOiI3NTYwMzA4NDY0NTUxMjE1MDg4Iiwic2VuZGVyVWlkIjoidV9lY2ZVV1ZVajNnN2hxeHBfZHpUbG9BIn0%3D&client=qqclient_online]]

## 1.2.云效

代码仓库：[[[zero-one-12psi · Codeup](https://codeup.aliyun.com/zero-one-star/zero-awei/zero-one-12psi)]]

项目小组-f1：[[[项目概览](https://devops.aliyun.com/projex/project/cdb95a1483f88f2b656a95ad9e)]]

# 2.资料

## 2.1.糕鸭果阮喵喵的文档

11-智慧社区-初始化：[[[5-11 统一用户名、全局环境准备、安装开发软件 | 阮喵喵的01星球笔记](https://01s-doc.ruan-cat.com/11comm/meeting-minutes/2025-5-11/)]]

12-进销存： [[https://01s-doc.ruan-cat.com/12psi/]]

# 3.准备

参考：[[[12 进销存 | 阮喵喵的01星球笔记](https://01s-doc.ruan-cat.com/12psi/#%E6%9C%AC%E6%AC%A1%E9%A1%B9%E7%9B%AE%E6%88%91%E9%9C%80%E8%A6%81%E7%9A%84%E5%88%B0%E7%9A%84%E6%9D%83%E9%99%90)]]

![[Pasted image 20251014153243.png]]

## 3.1.NodeJS

版本控制工具：`nvm` 管理 `node` 版本、nvm-desktop、fnm

针对node，参考阿伟学长的教程文档：documents\04、帮助文档\03、Nodejs安装.pdf

### 3.1.1.`nvm` 管理 `node` 版本

#### 3.1.1.1. `nvm`

`node` 的版本管理工具，在下载安装之前需要**先卸载 `node`。

#### 3.1.1.2.安装地址

https://github.com/coreybutler/nvm-windows/releases

#### 3.1.1.3. 查看

版本号。

```shell
nvm -v
# 1.2.2
```

#### 3.1.1.4 基本命令

```shell
# 禁用node.js版本管理（不卸载任何东西）
nvm off

# 启用node.js版本管理
nvm on

# 安装node.js的命令，其中<version>是版本号，如：nvm install 8.12.0
nvm install <version>

# 卸载node.js的命令，卸载指定版本的node.js，当安装失败时卸载使用
nvm uninstall <version>

# 显示所有安装的node.js的版本
nvm ls

# 显示可以安装的所有node.js的版本
nvm list available

# 切换到使用指定的node.js版本
nvm use <version>

# 显示nvm版本
nvm -v

# 安装最新稳定版
nvm install stable
```

### 3.1.2. `nvm-desktop` 软件

参考：[[[1111mp/nvm-desktop: Node Version Manager Desktop - A desktop application to manage multiple active node.js versions.](https://github.com/1111mp/nvm-desktop)]]

下载地址：[[[Releases · 1111mp/nvm-desktop](https://github.com/1111mp/nvm-desktop/releases)]]

## 3.2. `NPM` 和 `PNPM`

针对node和pnpm，参考阿伟学长的教程文档：documents\04、帮助文档\03、Nodejs安装.pdf

## 3.3.`IDE`

可选：CodeBuddy、CN、Cursor、Trae

## 3.4. `.emmx` 后缀的文件

可选软件：亿图脑图MindMaster

## 3.5.修改 git 名称

参考：[[[及时修改 git 用户名 | 阮喵喵的01星球笔记](https://01s-doc.ruan-cat.com/attention/change-git-user-name)]]

[[https://notes.ruan-cat.com/git/git-change-username.html#%E6%9B%B4%E6%94%B9-git-%E7%94%A8%E6%88%B7%E5%90%8D]]

1 浅克隆 无其他分支 且默认写入到 【A-proj】文件夹
git clone --depth=1 https://codeup.aliyun.com/zero-one-star/zero-awei/zero-one-12psi.git A-proj

2 永久更改当前git项目的用户名
git config --local user.name f1-糕鸭果阮喵喵

3 命名规则： f1-*

4 vscode 的 Git Graph 插件：
https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph

# 4.参考项目

## 4.1. `reference-project`

步骤：

1 安装mysql

2 运行sql，初始化业务数据库

3 运行 bat 运行本地后端和本地的前端项目

## 4.2.前端参考项目

项目连接：[[[11comm/examples/01s-origin at dev · ruan-cat/11comm](https://github.com/ruan-cat/11comm/tree/dev/examples/01s-origin)]]

作用：了解到01星球预设的vite+vue3项目基架

# 5.项目前期准备

## 5.1.阅读须知

![[72c067bd42f79fff45c2d89ef746149f.png]]

![[a0c76aeb02a43131628941105d6fb281.png]]

这些是开发文档，包括vue2源码，项目代码文档，这些在15 16号的时候看吧。

![](file:///C:\Users\WuHongyan\Documents\Tencent%20Files\1271736670\nt_qq\nt_data\Pic\2025-10\Ori\d584932fb4402e5fb496cc2ecf8d6e3e.png)  

  
![](file:///C:\Users\WuHongyan\Documents\Tencent%20Files\1271736670\nt_qq\nt_data\Pic\2025-10\Ori\47d4997bb5bce93cbbcbe7ef18478b49.png)





