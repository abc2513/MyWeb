# Git学习笔记

## 简要手册

```bash
#将代码上传到master分支（旧）
1.git init       //工作空间创建.git文件夹（默认隐藏了该文件夹）
2.git add .      //添加到暂存区
3.git commit -m "你的提交注释注释"
4.git remote add origin http://xxxxxxxxx.git   //本地仓库和远程github关联
5.git pull --rebase origin master  //远程有readme.md，拉一下
6.git push -u origin master        //代码合并
--------------------------------------------------------------------
#1.将代码上传到GitHub的默认main分支（新）
1.git --version    #查看版本
2.git config --global init.defaultBranch main   #git在2.28.0上，重新设置git默认分支为main

#2.在执行提交操作即可
1.git init       //工作空间创建.git文件夹（默认隐藏了该文件夹）
2.git add .      //添加到暂存区
3.git commit -m "更新"
4.git remote add origin git@github.com:abc2513/-.git   //本地仓库和远程github关联
5.git pull --rebase origin main  //远程有readme.md，拉一下
6.git push -u origin main        //代码合并
```

master

```bash
git remote add origin git@github.com:xxxx/xxx.git
git branch -M master
git push -u origin master
```



## 1 概述

分布式版本控制工具，本地库+远程库（GitLab、GitHub、Gitee）

工作区、暂存区、本地库、远程库

## 2 安装

不能有中文路径

## 3 常用命令

注：Linux命令均可用

### 3-1 设置用户签名

1. 设置用户签名`git config --global user.name 用户名` 
1. 设置用户邮箱(不会验证)`git config --global user.email 邮箱` 

注：签名与代码托管中心的账号没有任何关系

### 3-2 初始化本地化库

1. 初始化本地化库 ` git init`

### 3-3 查看本地库状态

1. 查看本地库状态`git status`

### 3-4 添加至暂存区

1. 新增文件 `git add hello_world.txt`
1. 删除暂存区文件`git rm --cached hello.txt`

### 3-5 提交至本地库

1. 提交至本地库(并形成历史版本)` git commit -m "日志信息" 文件名`
2. 查看版本信息(前七位版本号)` git reflog`
3. 查看详细版本信息（完整版本号）` git log`

### 3-6 修改文件

1. 修改文件`vim hello.txt`
2. 查看文件`cat hello.txt`

### 3-7 历史版本与版本穿梭

1. 查看引用版本信息(前七位版本号)` git reflog`
2. 查看详细版本信息（完整版本号）` git log`
3. 版本穿梭 `git reset --hard 引用版本信息`

底层是移动head指针

## 4 Git分支操作

### 4-1 什么是分支

1. 同时推进多个任务，为每个任务创建单独分支，分支的改变不影响主线运行

### 4-2 分支的好处

1. 同时并行推进多个功能开发，提高开发效率
2. 可以删除开发失败的分支

### 4-3 分支的操作

1. 查看当前分支`git branch -v`
2. 创建分支`git branch 分支名`
3. 切换分支分支`git checkout 分支名`
4. 合并指定分支到当前分支`git merge 分支名`

#### 冲突合并

冲突合并(同一位置两种修改导致,CONFLICT: Merge conflict in xxx),需要手动合并。

1. 手动打开文件`vim 文件名`，删除冲突内容和特殊字符，wq保存
2. 添加到暂存区`git add 文件名`
3. 执行提交`git commit -m "冲突合并"`==(没有文件名)==

合并只会改变当前分支

## 5 Git团队协助机制

### 5-1 团队内协助

代码托管中心

远程库--clone--》本地库--push--》远程库

远程库--pull--》本地库--push--》远程库

### 5-2 垮团队协助

远程库--fork--》远程库2--clone--》本地库--push--》远程库2--pull request--审核--merge--》远程库

## 6 GitHub操作

### 6-1 创建远程仓库(create a new repository )

### 6-2 远程仓库操作

- 查看当前所有远程仓库别名 `git remote -v`
- 为远程仓库起别名 `git remote add 别名 地址仓库` (建议别名和仓库名称一致)
- 推送分支到远程服务器`git push 远程仓库别名 分支`(~~弹窗选择浏览器账号或输入口令登录，该方法已经过时~~)
- 拉取远程分支`git pull 远程仓库别名 分支`
- 克隆远程分支` git clone 仓库地址`（克隆公有库无需登录账号）（克隆=初始化本地仓库+拉取代码+创建别名）
- 邀请成员（Manage access）（github操作）

### 6-3 跨团队协助

https://www.bilibili.com/video/BV1vy4y1s7k6?p=25&vd_source=b3a61f4abb2aa7da517f6cf364e96fdd

### 6-4 SSH免密登录

1. 在`C://users/?/`里打开git bush输入`ssh-keygen -t rsa -C 邮箱`
2. 回车三次
3. 打开`C://users/?/.ssh/id_rsa.pub`复制公钥
4. 账号设置内添加SSH公钥

## 7 IDEA集成Git

### 7-1 配置git忽略文件

语法

```
空格不匹配任意文件，可作为分隔符，可用反斜杠转义
开头的文件标识注释，可以使用反斜杠进行转义
! 开头的模式标识否定，该文件将会再次被包含，如果排除了该文件的父级目录，则使用 ! 也不会再次被包含。可以使用反斜杠进行转义
/ 结束的模式只匹配文件夹以及在该文件夹路径下的内容，但是不匹配该文件
/ 开始的模式匹配项目跟目录
如果一个模式不包含斜杠，则它匹配相对于当前 .gitignore 文件路径的内容，如果该模式不在 .gitignore 文件中，则相对于项目根目录
** 匹配多级目录，可在开始，中间，结束
? 通用匹配单个字符
* 通用匹配零个或多个字符
[] 通用匹配单个字符列表

###栗子

bin/: 忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
/bin: 忽略根目录下的bin文件
/*.c: 忽略 cat.c，不忽略 build/cat.c
debug/*.obj: 忽略 debug/io.obj，不忽略 debug/common/io.obj 和 tools/debug/io.obj
**/foo: 忽略/foo, a/foo, a/b/foo等
a/**/b: 忽略a/b, a/x/b, a/x/y/b等
!/bin/run.sh: 不忽略 bin 目录下的 run.sh 文件
*.log: 忽略所有 .log 文件
config.php: 忽略当前路径的 config.php 文件
```



创建忽略规则文件`xxx.ignore`，建议为`git.ignore`

```gitignore
#注释
*.class
```

idea模板

```gitignore
# Compiled class file 类
*.class

# Log file 日志
*.log

# BlueJ files 
*.ctxt

# Mobile Tools for Java (J2ME) 
.mtj.tmp/# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

hs_err_pid*

.classpath
.project
.settings
target
.idea
*.iml
```

在.gitconfig文件中引用忽略配置文件（在windows家目录下）

```
[core]
	excludesfile = C:/User/asus/git.ignore
```

windows默认是反斜线\，复制路径需要改成正斜线

## 8 IDEA集成GitHub

## 9

## 10 自建代码托管平台（GitLab）

### 10-1 简介

### 10-2 官网地址

### 10-3 安装

EE为企业版，CE为社区版（免费）

## Bonobo Git Server









