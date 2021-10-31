---
title: ssh连接虚拟机
date: 2021-10-31 09:25:53
categories: forensics
tags: secure
---

# ssh连接虚拟机

> [ssh相关操作](https://blog.csdn.net/java_dotar_01/article/details/76942563)



## 配置文件

> vim中输入`/`可以进行搜索

```
vim /etc/ssh/sshd_config
```

将 `PasswordAuthentication` 改为 `yes`

![image-20201004094841713](image-20201004094841713.png)

将上图的**no**,改成**yes**



> 为防止使用root登录时发生permission denied的情况

将**PermitRootLogin no(服务端 SSH 服务配置了禁止root用户登录策略)**的注释取消掉,并改成**yes**

> 按需要重启ssh服务,一般不用

```
service sshd restart
```

对于centos,重启命令有点不一样

```
systemctl restart sshd.service
```



## 配置网络

1. `ifconfig`

   查看虚拟机IP地址

   ![Snipaste_2020-10-04_09-35-19](Snipaste_2020-10-04_09-35-19.png)

2. 虚拟机->编辑->虚拟网络编辑器

   ![image-20201004095046401](image-20201004095046401.png)

3. 修改

   ![image-20201004095132187](image-20201004095132187.png)

   ==注意这里的子网IP需要设置的和虚拟机所在IP一致！！==

   ![image-20201004095201304](image-20201004095201304.png)



## ssh连接

cmd中输入

```
ssh root@192.168.217.131 (-p xxxx)
```



## scp远程复制

如果ssh能连接上，就可以用这个命令进行复制

- 复制文件

  ```
  scp root@192.168.184.129:/home/admin888/fund/db.sqlite3 C:\Users\mercer\Desktop
  ```

- 复制文件夹

  ```
  scp -r root@192.168.184.129:/home/admin888/fund/ C:\Users\mercer\Desktop
  ```

  ![image-20211021222907451](image-20211021222907451.png)



## 一些报错的解决方法

### ping不通

> 回寝室后又尝试使用ssh,但失败了,说连接超时

首先,尝试在主机ping了一下虚拟机,发现失败了

虚拟机中ifconfig一下

主机中ipconfig一下

![image-20210714163016755](image-20210714163016755.png)

综上发现两个的子网不一样,也就是说不在同一个网段

所以**编辑**->**虚拟网络编辑器**->**VMnet8**->修改子网,使其在同一网段

![image-20201009145400504](image-20201009145400504.png)

![image-20201009145513559](image-20201009145513559.png)

这里改成`192.168.137.0` 

这时就ping的通了



### 端口错误

```
netstat -atulnp
```

![image-20211015092032969](image-20211015092032969.png)

发现ssh服务开在7001端口!!!!

长安杯20的时候被这个端口坑的不轻

![image-20211015092125689](image-20211015092125689.png)



其实在配置页中也可以看到端口信息

![image-20211018191835239](image-20211018191835239.png)



### Permission Denied

![image-20211018192752599](image-20211018192752599.png)

后来发现是将 `PasswordAuthentication` 改为 `yes` 后,没保存

所以不能通过输入密码登入
