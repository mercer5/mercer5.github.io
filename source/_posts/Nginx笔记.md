---
title: Nginx笔记
date: 2021-10-31 12:46:03
categories: forensics
tags: secure
---

# Nginx笔记

> [Nginx 配置文件解析](https://www.runoob.com/note/34639)

## 找到配置文件

```
locate nginx.conf
```

一般在`/etc/nginx/nginx.conf`



## 重要的配置指令

- user: worker进程的用户和组

- error_log: 错误写入文件

- pid: 记录主进程ID的文件

  

## include文件

增强配置文件的可读性(就是用来引入其他配置文件的?)



## location部分

### root

> 也可以设置在server部分

设置网站的根目录,eg

```
root /var/www/html/
```

假如域名是

```
example.com
```

那么访问`example.com/folder/pic.jpg`即使访问`/var/www/html/folder/pic.jpg`



### index

> 也可以设置在server部分

设置主页名字,一般为

```
index, index.html, index.htm, index.php
```

作用: 访问`example.com/folder/`时,自动依次检索folder目录下的这些文件,一旦找到就返回给用户



## server部分

虚拟服务器部分

###  listen

用于指定监听端口,也就是对外开放的端口

定义了一个IP地址/端口组合或是UNIX域套接字路径

```
listen address [:port]
listen port
listen unix:path
```



### server_name

指明虚拟主机的名字,用于选择不同的虚拟主机

默认值为: "",对于没有设置Host头字段的请求,会匹配到该server处理

这个字段的设置也可以是IP地址,用于IP直接访问时



### location

`location` 有多种功能, 重定向或提供来自客户端的url。 

它可以有多个，用来匹配用户请求的地址中 `server_name` 后面的部分。

1. 反向代理就会用到这个，例如在反代 deluge-web 时：

   ```
   location /deluge/
   {
     proxy_pass http://localhost:8112/;
     proxy_set_header X-Deluge-Base "/deluge/";
     add_header X-Frame-Options SAMEORIGIN;	
   }
   ```

   我们先看这段配置的第一行： `location /deluge/` 。这表示它会匹配对 example.com/deluge/ 以及子目录的所有请求。

   里面的第一句 `proxy_pass http://localhost:8112/;` 是反代的核心。它表示，把匹配到的所有请求都转发到 `http://localhost:8112/` 上，从而使外界访问 example.com/deluge 的时候能看到在服务器上访问 `http://localhost:8112/` 所看到的页面。

   而后面的两句就和 HTTP 请求头有关系了。前面讲到 HTTP 请求头会包含用户请求的域名，事实上它除此之外还有用户的 IP、请求方法、Cookie、加密算法等信息。直接的 `proxy_pass` 会使后端服务器接收到的请求头与用户实际发出的请求头不同，所以可能需要加上一些额外的语句。

2. `location` 里面也可以不是 `proxy_pass` ，它还可以是 `root` 、 `index` 来使用不同的根目录

3. `location` 的第一行匹配还有别的方式，例如 `location = /a/` 。

   location匹配结尾是否加斜杠“/”与其中的 `root` 或 `proxy_pass` 结尾是否加反斜杠之间有一定关系，不同组合有不同效果。



## http部分

### access_log

> 能够出现在 `http` 、 `server` 、 `location` 中。
>
> 可以在 `server` 中打开，而在某些特定的 `location` 中关闭。

用于记录访问日志。一般写法：

```
access_log /var/log/thelogfile.log;
```

或者关闭之：

```
access_log off;
```



## 错误日志文件格式

```
<timestamp> [log-level] <master/worker pid>#0: message
```

error级别的message包括

- 连接数
- 系统调用
- 系统调用参数
- 从系统调用产生的错误消息
- 造成错误请求的客户端
- 负责处理请求的server
- 请求本身
- 在请求中发送的host头



## 作用

https://zhuanlan.zhihu.com/p/54793789

### 静态HTTP服务器

首先，Nginx是一个HTTP服务器，可以将服务器上的静态文件（如HTML、图片）通过HTTP协议展现给客户端。

配置：

```text
server {
listen80; # 端口号
location / {
root /usr/share/nginx/html; # 静态文件路径
}
}
```

### 反向代理服务器

客户端本来可以直接通过HTTP协议访问某网站应用服务器，网站管理员可以在中间加上一个Nginx，客户端请求Nginx，Nginx请求应用服务器，然后将结果返回给客户端，此时Nginx就是反向代理服务器。

配置：

```text
server {
listen80;
location / {
proxy_pass http://192.168.20.1:8080; # 应用服务器HTTP地址
}
}
```

既然服务器可以直接HTTP访问，为什么要在中间加上一个反向代理，不是多此一举吗？反向代理有什么作用？继续往下看，下面的负载均衡、虚拟主机等，都基于反向代理实现，当然反向代理的功能也不仅仅是这些。

### 负载均衡

当网站访问量非常大，网站站长开心赚钱的同时，也摊上事儿了。因为网站越来越慢，一台服务器已经不够用了。于是将同一个应用部署在多台服务器上，将大量用户的请求分配给多台机器处理。同时带来的好处是，其中一台服务器万一挂了，只要还有其他服务器正常运行，就不会影响用户使用。

Nginx可以通过反向代理来实现负载均衡。

配置: (将请求轮询分配到应用服务器，也就是一个客户端的多次请求，有可能会由多台不同的服务器处理。)

```text
upstream myapp {
server192.168.20.1:8080; # 应用服务器1
server192.168.20.2:8080; # 应用服务器2
}

server {
listen80;
location / {
proxy_pass http://myapp;
}
}
```

配置：(通过ip-hash的方式，根据客户端ip地址的hash值将请求分配给固定的某一个服务器处理。)

```text
upstream myapp {
 ip_hash; # 根据客户端IP地址Hash值将请求分配给固定的一个服务器处理
 server192.168.20.1:8080;
 server192.168.20.2:8080;
 }
 server {
 listen80;
 location / {
 proxy_pass http://myapp;
 }
 }
```

配置：(服务器的硬件配置可能有好有差，想把大部分请求分配给好的服务器，把少量请求分配给差的服务器，可以通过weight来控制。)

```text
upstream myapp {
server192.168.20.1:8080 weight=3; # 该服务器处理3/4请求
server192.168.20.2:8080; # weight默认为1，该服务器处理1/4请求
 }
server {
listen80;
location / {
proxy_pass http://myapp;
 }
 }
```

### 虚拟主机

有的网站访问量大，需要负载均衡。然而并不是所有网站都如此出色，有的网站，由于访问量太小，需要节省成本，将多个网站部署在同一台服务器上。

例如将`http://www.aaa.com`和http://www.bbb.com两个网站部署在同一台服务器上，两个域名解析到同一个IP地址，但是用户通过两个域名却可以打开两个完全不同的网站，互相不影响，就像访问两个服务器一样，所以叫两个虚拟主机。

配置：

```text
server {
 listen 80 default_server;
 server_name _;
 return 444; # 过滤其他域名的请求，返回444状态码
 }
 server {
 listen 80;
 server_name www.aaa.com; # www.aaa.com域名
 location / {
 proxy_pass http://localhost:8080; # 对应端口号8080
 }
 }
server {
listen 80;
server_name www.bbb.com; # www.bbb.com域名
location / {
proxy_pass http://localhost:8081; # 对应端口号8081
 }
 }
```

在服务器8080和8081分别开了一个应用，客户端通过不同的域名访问，根据server_name可以反向代理到对应的应用服务器。

虚拟主机的原理是通过HTTP请求头中的Host是否匹配server_name来实现的

另外，server_name配置还可以过滤有人恶意将某些域名指向你的主机服务器。

### FastCGI

Nginx本身不支持PHP等语言，但是它可以通过FastCGI来将请求扔给某些语言或框架处理（例如PHP、Python、Perl）。

```text
server {
listen 80;
location ~ \.php$ {
include fastcgi_params;
fastcgi_param SCRIPT_FILENAME /PHP文件路径$fastcgi_script_name; # PHP文件路径
fastcgi_pass127.0.0.1:9000; # PHP-FPM地址和端口号
# 另一种方式：fastcgi_pass unix:/var/run/php5-fpm.sock;
}
}
```

配置中将.php结尾的请求通过FashCGI交给PHP-FPM处理，PHP-FPM是PHP的一个FastCGI管理器。


