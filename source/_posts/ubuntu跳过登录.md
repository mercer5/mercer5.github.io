---
title: ubuntu跳过登录
date: 2021-10-31 09:50:34
categories: forensics
tags: secure
---

# ubuntu跳过登录

## 常规方法

> 以chagnan2019的检材4中的虚拟机为例

1. 开机长按`esc`,进入`advanced options for ubuntu`

![image-20210717135201696](image-20210717135201696.png)

2. 选中`recovery mode`,按`e`进入命令行模式

![image-20210717135254135](image-20210717135254135.png)

3. 找到有recovery nomodeset的行，**删除**`recovery nomodeset` ，并在本行末尾**加上**`quiet splash rw single init=/bin/bash` ，按F10；

![image-20210717135936486](image-20210717135936486.png)

![image-20210717140418626](image-20210717140418626.png)

4. 在命令行输入passwd 用户名，修改密码

![image-20210717140527140](image-20210717140527140.png)

​		这里我将用户root的密码修改成了123456,这样下次就可以直接进入了



## 火眼仿真绕过

放入虚拟机的vmdk文件

![image-20211021214423156](image-20211021214423156.png)

![image-20211021214653320](image-20211021214653320.png)