---
tags:
  - git
date: 2025-04-27
---
# 0 版本查询

```shell
git --version
```

# 3 Git 安装与常用命令

### 3.1.2 基本配置

1. 打开 `Git Bash`

2. 设置用户信息

```shell
git config --global user.name “Jessica0706”
git config --global user.email “1271736670@qq.com”
```

3. 查看配置信息

```shell
git config --global user.name
git config --global user.email
```

### 3.1.3 为常用指令配置别名（可选）

[[Git讲义.pdf#page=4&selection=47,0,49,13|Git讲义, 页面 4]]

用户目录：C:\Users\WuHongyan

```shell
touch ~/.bashrc
// ~表示当前文件夹的根目录
```

在 .bashrc 文件中输入以下内容（可使用 VSCode 打开）

```shell
#用于输出git提交日志
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'

#用于输出当前目录所有文件及基本信息
alias ll='ls -al'
```

### 3.1.4 解决 GitBash 乱码问题

```shell
git config --global core.quotepath false
```

Q: 如何找到 `${git_home}`？

```
// 打开cmd，输入where git
```

再在 `${git_home}/etc/bash.bashrc` 文件的最后两行输入以下代码

```
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
```

## 3.3 基础操作指令

![[Pasted image 20250429214134.png]]

![[Pasted image 20250429214200.png]]

添加文件到暂存区。

```shell
#  方法1：指定文件
git add file01.txt

# 方法2：全部文件
git add .
```

![[Pasted image 20250429214422.png]]

将暂存区里的文件添加到仓库里。

```shell
git commit -m "add file01"
// m是message的缩写
// ""内是注释
```

![[Pasted image 20250429215110.png]]

查看文件提交到那个地方？查看历史记录（日志）。

```shell
git log
git log --pretty=oneline --abbrev-commit --graph --decorate
// Mac本需要加上--decorate
```

### 3.3.4 查看提交日志（log）

![[Pasted image 20250429220705.png]]

![[Pasted image 20250429215308.png]]

修改 file 01.txt 文件。

```
vi file01.txt
```

```
// 输入 i/a/o/O 进入编辑模式
update count=1
// 按下Esc退出编辑模式
// 接着连按两次大写字母Z；或者在半角模式下输入:wq
```

![[Pasted image 20250429215741.png]]

将文件再次放入暂存区及仓库。

![[Pasted image 20250429220450.png]]

查看帮助文档。

```shell
git help<命令>
```

### 3.3.5 版本回退。

```shell
git reset --hard commitID
```

在 Git 中，若选中字符，则默认已拷贝。不要按 ctrl + C ，在命令行中，其代表“结束”。

黏贴：按住鼠标滚轮。或者鼠标右键。

![[Pasted image 20250429221727.png]]

查看已经删除的记录。

```shell
git reflog
```

![[Pasted image 20250429221835.png]]

### 3.3.6 添加文件至忽略列表

在工作目录中创建.gitignore 文件，列出要忽略的文件模式。

```shell
touch file02.a
touch .gitignore
```

```
*.a
// 所有后缀为a的文件
```

![[Pasted image 20250429222346.png]]

## 3.4 分支

把工作从主线开发主线上分离开来。

### 3.4.1 查看本地分支

```shell
git branch
```

![[Pasted image 20250430092127.png]]

### 3.4.2 创建本地分支

```shell
git branch <分支名>
```

![[Pasted image 20250430092313.png]]

### 3.4.4 切换分支（checkout）

```shell
// 切换到已存在的分支
git checkout <分支名>

// 切换到不存在的分支（创建并切换）
git checkout -b <分支名>
```

切换到 dev 01 分支。

![[Pasted image 20250430092808.png]]

![[Pasted image 20250430092839.png]]

切换到 master 分支。

![[Pasted image 20250430092935.png]]

![[Pasted image 20250430092942.png]]

创建并切换分支。

![[Pasted image 20250430093059.png]]

### 3.4.6 合并分支（merge）

```shell
git merge <分支名>
```

在分支 dev 01 中建立文件 file 03.txt

![[Pasted image 20250430093439.png]]

切换到 master 文件，发现没有 file 03.txt 文件。

![[Pasted image 20250430093625.png]]

进行合并前，先切换到 master（主线），再合并。

![[Pasted image 20250430093914.png]]

### 3.4.7 删除分支

不能删除当前分支，只能删除其他分支.

```shell
// 删除分支时，需要做各种检查
git branch -d <分支名>

// 不做任何检查，强制删除
git branch -D <分支名>
```

### 3.4.8 解决冲突

两个分支上对文件的修改可能会存在冲突，如同时修改了同一个文件的同一行。

解决步骤：

1. 处理文件中冲突的地方；
2. 将解决完冲突的文件加入暂存区（add）；
3. 提交到仓库（commit）；

![[Pasted image 20250430094707.png]]

### 3.4.9 开发中分支使用原则与流程

- master（生产）分支
	线上分支，主分支，中小规模项目作为线上运行的应用对应的分支。

- develop（开发）分支
	是从 master 创建的分支，一般作为开发部门的主要开发分支，若没有其他并行开发不同期上线要求，都可以在此版本中进行开发，阶段开发完成后，需要合并到 master 分支中，准备上线。

- feature/xxxx 分支
	从 develop 创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到 develop 分支。

- hotfix/xxxx 分支
	从 master 派生的分支，一般作为线上 bug 修复使用，修复完成后需要合并到 master、test、develop 分支。

- 其他分支
	如 test 分支（用于代码测试）、pre 分支（预上线分支）等等。

![[Pasted image 20250430095726.png]]

# 4 Git 远程仓库

## 4.1 常用的托管服务 [远程仓库]

**常用**

GitHub：[[https://github.com/]]

码云：[[https://gitee.com/ ]]

GitLab（需要自主部署）：[[https://about.gitlab.com/ ]]

## 4.4 配置 SSH 公钥

- 生成 `SSH` 公钥

```shell
ssh-keygen -t rsa

// 不断回车
// 若公钥已经存在，则自动覆盖
```

![[Pasted image 20250501170115.png]]

- `Gitee` 设置账户公钥

```shell
// 获取公钥
cat ~/.ssh/id_rsa.pub

// 验证是否配置成功
ssh -T git@gitee.com
```

![[Pasted image 20250501170528.png]]

## 4.5 操作远程仓库

### 4.5.1 添加远程仓库

先初始化本地库，然后与已创建的远程库进行对接。

```shell
git remote add <远端名称> <仓库路径>
// 远端名称：默认是origin，取决于远端服务器设置
// 仓库路径：从远端服务器获取此URL
```

![[Pasted image 20250501180302.png]]

p.s. 注意，此处为 SSH 的地址，不要误选成 HTTPS 的。

![[Pasted image 20250501180600.png]]

### 4.5.2 查看远程仓库

```shell
git remote
```

![[Pasted image 20250501171208.png]]

### 4.5.3 推送到远程仓库

```shell
git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名]]

// 若远程分支名和本地分支名称相同，则可以致谢本地分支
git push origin master

// -f 表示强制覆盖

// --set-upstream 推送到远端的同时并建立起和远端分支的关联关系
git push --set-upstream origin master

// 若当前分支已经和远端分支关联，则可以省略分支名和远端名
// master:master
// git push 将 master 分支推送到已关联的远端分支
```

![[Pasted image 20250501180641.png]]

![[Pasted image 20250501204927.png]]

--set-upstream 的作用如下。

![[Pasted image 20250501205633.png]]

### 4.5.4 本地分支与远程分支的关联关系

```shell
// 查看关联关系
git branch -vv
```

![[Pasted image 20250501205425.png]]

### 4.5.5 从远程仓库克隆

若已有一个远端仓库，则可直接 clone 到本地。

```shell
git clone <仓库路径> [本地目录]
# 仓库路径需要复制SSH的
# 本地目录可以省略，会自动生成一个目录，其名称与远端仓库的名称相同
```

![[Pasted image 20250501210239.png]]

### 4.5.6 从远程仓库中抓取和拉取

先从远端仓库里的更新都下载到本地后，再进行 merge 操作。

```shell
# 抓取命令：将仓库里的更新都抓取到本地，不会进行合并
# 若不指定远端名称和分支名，则抓取所有分支
git fetch [remote name] [branch name]

# 拉取命令：将远端仓库的修改拉到本地并自动进行合并，等同于 fetch + merge
# 若不指定远端名称和分支名，则抓取所有并更新当前分支
git pull [remote name] [branch name]
```

抓取命令演示。

![[Pasted image 20250501214351.png]]

![[Pasted image 20250501214723.png]]

拉取命令演示。

![[Pasted image 20250501215052.png]]

### 4.5.7 解决合并冲突

![[Pasted image 20250501215913.png]]

问题：A 先修改了代码，B 在 push 之前要先 pull，但由于 A push 的代码和 B 在本地工作区的代码修改的是同一位置，此时 pull 指令发生了冲突，需要合并。

方法：同解决本地分支冲突相同。（先 pull 再 push）

# 5 在 Ideal 中使用 Git

## 5.2 在 Ideal 中操作 Git

场景：本地已有一个非 Git 项目，并需要将该项目放入码云的仓库中，与其他人一起协作开发。

### 5.2.1 创建项目远程仓库

参照 4.3

### 5.2.2 初始化本地仓库

在确保文件内有 .gitignore 文件后，再进行如下操作。（新版 Ideal 会自带）

![[Pasted image 20250501225006.png]]

![[Pasted image 20250501225148.png]]

### 5.2.4 提交到本地仓库

选择“提交”。

### 5.2.6 推送到远程仓库

选择“推送”。

### 5.2.7 克隆远程仓库到本地

![[Pasted image 20250501231009.png]]

## 5.3 Ideal 常用 Git 操作入口

![[Pasted image 20250501231420.png]]

![[Pasted image 20250501231432.png]]

新版 Ideal 有所区别。

# 6 在 SourceTree 中使用 Git

## 6.1 常用指令

## 6.2 刷新远端分支到本地

```shell
git remote update
```

# 7 完整本地初始化后与 Gitee 远端仓库连接

## 7.1 完整本地初始化

```shell
git init && git add . && git commit -m "Initial commit"
```

## 7.2 在 Gitee / GIthub 新建远端仓库

## 7.3 连接本地和远端仓库

打开「Git Bash Here」

```shell
git remote add origin 远程仓库HTTPS链接
```

如果远端已有代码，则先拉取。

```shell
git pull --rebase origin master
```

最后，推送代码到远端。

```shell
git push -u origin master
```

# 附

## 要点 1

**切换分支前先提交本地代码。**

## IDEA 集成 GitBash 作为 Terminal

![[Pasted image 20250501232008.png]]

## 指令速查（待添加）

```git

```

