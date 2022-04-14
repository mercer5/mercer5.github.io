---
title: xctf-web-新手练习区
date: 2020-08-25 08:37:15
categories: ctf
tags: 
- ctf
- secure
---

## view_source

![image-20200716153232985](image-20200716153232985.png)



## get_post

![image-20200716153348036](image-20200716153348036.png)



## robots

> robots是网站跟爬虫间的协议，用简单直接的txt格式文本方式告诉对应的爬虫被允许的权限，也就是说robots.txt是搜索引擎中访问网站的时候要查看的第一个文件。当一个搜索蜘蛛访问一个站点时，它会首先检查该站点根目录下是否存在robots.txt，如果存在，搜索机器人就会按照该文件中的内容来确定访问的范围；如果该文件不存在，所有的搜索蜘蛛将能够访问网站上所有没有被口令保护的页面。

![image-20200716153539061](image-20200716153539061.png)

![image-20200716153602077](image-20200716153602077.png)



## backup

> php的备份有两种：.php~和.php.bak

![image-20200716154007563](image-20200716154007563.png)



## cookie

![image-20200716154154950](image-20200716154154950.png)

![image-20200716154219258](image-20200716154219258.png)

![image-20200716154236015](image-20200716154236015.png)



## disabled_button

![image-20200716154347970](image-20200716154347970.png)

删了下划内容

![image-20200716154418626](image-20200716154418626.png)



## weak_auth

爆破

123456



## command_execution

> | 的作用为将前一个命令的结果传递给后一个命令作为输入
>
> &&的作用是前一条命令执行成功时，才执行后一条命令
>
> ![image-20200716155604029](image-20200716155604029.png)

1. 输入`127.0.0.1 |  find / -name "flag.txt" `

   ![image-20200716155446460](image-20200716155446460.png)

2. 输入`127.0.0.1 |  cat /home/flag.txt`

   ![image-20200716155526247](image-20200716155526247.png)



## simple_php

![image-20200716155940510](image-20200716155940510.png)

1. `a==0 && a 存在`: **php弱类型**

2. `is_numeric `:检测变量是否为数字或数字字符串 

   所以b不是数字或数字字符串但>1234

   **php弱类型比较**

   可用`9999`+`任意字符`



## xff_referer

> X-Forwarded-For:简称XFF头，它代表客户端，也就是HTTP的请求端真实的IP，只有在通过了HTTP 代理或者负载均衡服务器时才会添加该项
>
> HTTP Referer是header的一部分，当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器我是从哪个页面链接过来的

![image-20200716161916503](image-20200716161916503.png)



## webshell

蚁剑/菜刀

![image-20200716162150632](image-20200716162150632.png)

![image-20200716162232973](image-20200716162232973.png)

![image-20200716162240177](image-20200716162240177.png)



## simple_js

1.打开页面，查看源代码，可以发现js代码，如图所示。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/simple_js/1.png)

2.进行代码审计，发现不论输入什么都会跳到假密码，真密码位于 fromCharCode 。

3.先将字符串用python处理一下，得到数组[55,56,54,79,115,69,114,116,107,49,50]，exp如下。

```python
s="\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30"
print (s)
```

4.将得到的数字分别进行ascii处理，可得到字符串786OsErtk12，exp如下。

```python
a = [55,56,54,79,115,69,114,116,107,49,50]
c = ""
for i in a:
    b = chr(i)
    c = c + b
print(c)
```

5.规范flag格式，可得到Cyberpeace{786OsErtk12}