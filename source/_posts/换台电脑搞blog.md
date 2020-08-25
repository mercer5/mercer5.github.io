---
title: 换台电脑搞blog
date: 2020-08-25 08:31:35
categories: blog
tags: blog
---

# 换台电脑搞blog

> 很好,之前的预言中了,电脑重装后,啥都没了
>
> 从现在开始重新搭建blog所需的环境之类的东西
>
> 顺便记录一下,恢复过程
>
> 2020-8-24

## 下载

1. git
2. nodejs



## 连接

- 设置git全局邮箱和用户名

```
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

- 设置ssh key

```
ssh-keygen -t rsa -C "youremail"
#生成后填到github
```

- 安装hexo

```
sudo npm install hexo-cli -g
```

但是已经不需要初始化了，

直接在任意文件夹下，

```
git clone git@………………
```

然后进入克隆到的文件夹：

```
cd xxx.github.io
npm install hexo-deployer-git --save
```

生成，部署：

```
hexo g
hexo d
```

然后就可以开始写你的新博客了

```
hexo new newpage
```

不要忘了，每次写完最好都把源文件上传一下

```
git add .
git commit –m "xxxx"
git push
```