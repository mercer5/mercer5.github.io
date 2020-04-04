---
title: 如何搭建blog
date: 2020-2-10 17:15:51
categories: blog
tags: 
- blog

---

# 如何搭建blog

## 使用软件

**本人系统win10**

Node.js,Git,hexo,sublimetext.

## 环境搭建

### 安装

Hexo 基于 Node.js，搭建过程中还需要使用 npm（Node.js 已带） 和 git

因此先搭建本地操作环境，安装 Node.js 和 Git。

- Node.js:https://nodejs.org/zh-cn/
- Git:https://git-scm.com/download/win

一路点击**下一步**

### 检查

Win+R 输入 cmd 

输入 node -v

输入npm -v

输入git --version 

若分别出现各自的版本号,就ok了.如图

![1](1.jpg)

## 连接github

### 注册github:

要想连接github,首先你要有: )

### 连接

- 打开git bash
- 设置用户名和邮箱

```
git config --global user.name "GitHub 用户名"
git config --global user.email "GitHub 邮箱"
```

- 创建ssh密钥

```
ssh-keygen -t rsa -C "GitHub 邮箱"           (一路回车就行)
```

![2](2.jpg)

- 添加密钥

进入**上图划绿线**的目录，用**记事本**打开公钥**id_rsa.pub**文件并复制里面的内容。

登陆 GitHub ，进入 Settings 页面，选择左边栏的 SSH and GPG keys，点击 New SSH key。

Title 随便取个名字，粘贴复制的 id_rsa.pub 内容到 Key 中，点击 Add SSH key 完成添加。

## 创建github pages 仓库

1. GitHub 主页右上角加号 -> New repository：

- Repository name 中输入**用户名.github.io**
- **勾选** “Initialize this repository with a README”
- Description **选填**

2. 填好后点击 Create repository 创建。

![3](3.jpg)

![4](4.jpg)

3. 创建后默认自动启用 HTTPS，博客地址为：**https://用户名.github.io**

## 本地安装 Hexo 博客程序

### 安装

- 打开git bash

- 使用 npm 一键安装 Hexo 博客程序：

```
npm install hexo-cli -g
```

### hexo 初始化和本地浏览

- 初始化并安装所需组件

win + R,     cmd,       回车

```
hexo init 博客名      # 初始化
cd 博客名             # 进入目录
npm install          # 安装组件
```

- 启动本地程序并浏览

```
hexo g   # 生成页面
hexo s   # 启动预览
```

浏览器中访问 `http://localhost:4000`，出现 Hexo 默认页面，本地博客安装成功！

## 部署 Hexo 到 GitHub Pages

### Sublime Text

1. 安装

-  Sublime Text:https://www.sublimetext.com/3
-  一路回车

2. 使用

- 打开后将blog所在文件夹往里面拖即可

### 部署

1. 安装 hexo-deployer-git：

```
npm install hexo-deployer-git --save
```

2. 在Sublime Text中改些配置

- **修改 _config.yml** 文件末尾的 Deployment 部分

```
deploy:
  type: git
  repo: git@github.com:用户名/用户名.github.io.git
  branch: master
```

repo可在github仓库直接复制

![5](5.jpg)

- **修改 _config.yml** 文件开头的 url 部分,改为自己blog地址

![6](6.jpg)

- **修改 _config.yml** 文件开头的 site 部分

  搜索关键词Site，如下：

```
# Site
title: Chloneda             #你的站点标题
subtitle: Less is more
description: Less is more   #你的站点描述
keywords: chloneda
author: chloneda            #站点作者
```



### 上传

传到github上

```
hexo d

```

### 查看

在浏览器中输入github域名`https://用户名.github.io`,就可以看到hexo网站了.



## next

1. 下载主题

2. 将主题文件拷贝至站点目录的 `themes` 目录

3. 打开 **站点配置文件**， 找到 `theme` 字段，并将其值更改为 `next`(就可以使用该主题了)

   ```
   # Extensions
   ## Plugins: https://hexo.io/plugins/
   ## Themes: https://hexo.io/themes/
   theme: next
   
   ```

   

