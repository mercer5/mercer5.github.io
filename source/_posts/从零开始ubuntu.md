---
title: 从零开始ubuntu
date: 2020-04-04 15:21:02
tags: 虚拟机
---
# 从零开始ubuntu 18.04



----

1. 投入kali怀抱好久了,突然想搞ubuntu

   一个搞人,一个搞编程,分工明确,嗯

   想想搞kali时的艰难经过,想必这次也不会例外

   所以记录一下使用ubuntu的曲折历程

2. 要是我的提供不了帮助,建议拉到最下面,点击链接看原博客

   每个人情况不一样,我只记录并解决了我所遇到的问题

   看着参考越来越多,相当于一个小型资料库了都: (

3. 记录一下时间,要是时间过去很久了,参考价值就小了

   2020-04-03(最近更新时间)

----



## 换源

```
sudo gedit /etc/apt/sources.list
```

附上清华源

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
```



## vim

```
sudo apt install vim
```

一些简单的操作

1. vim name.txt(打开或创建编辑器)

2. "i"插入模式(只有进入插入模式才可以更改内容)    (^u,好用的删行小技巧)

3. "esc"退出插入模式

4. ":",wq(写入并保存)

   (A：在最后输入命令时，直接输入"x"，也是一样的，即X=WQ。

   B：最快捷的方法：按了ESC后，直接按shift+zz，或者切换到大写模式按ZZ，就可以保存退出了，即是按2下大写的Z)



## pip

安装

```
sudo apt-get install python-pip
sudo apt-get install python3-pip
```

升级

```
sudo pip3 install --upgrade pip
```

卸载

```
sudo apt-get remove python3-pip
```



## vm-tools

电脑里的文件拖不进虚拟机,复制的也粘贴不了

输入命令行全手打,我天

不过这个不是ubuntu的锅,是vm的

```
sudo apt-get autoremove open-vm-tools
sudo apt-get install open-vm-tools
sudo apt-get install open-vm-tools-desktop
```



## gcc

安装

```
sudo apt-get install gcc
```

验证

```
gcc --version
```

使用

​	强烈推荐: http://c.biancheng.net/view/660.html

​	照着做就会了(为什么有种推销员的感觉???)



## 好玩的终端命令

**小火车**

```bash
sudo apt install sl
sl
```

**cmatrix代码雨**

```bash
sudo apt install cmatrix
cmatrix
```

**终端火焰**

```bash
sudo apt-get install libaa-bin
aafire
```



## 输入法

**安装Fcitx输入框架**

```
sudo apt install fcitx
```

**安装**

上搜狗输入法官网下载Linux版本搜狗输入法（32位和64位根据自己情况，在虚拟机上用浏览器下载

点击安装包

**设置**

![Snipaste_2020-04-02_09-31-10](Snipaste_2020-04-02_09-31-10.png)



根据红色箭头进入语言安装界面，安装语言（会自动安装中文语言）

![Snipaste_2020-04-02_09-31-18](Snipaste_2020-04-02_09-31-18.png)

根据下方箭头更爱输入框架为fcitx，然后点击上面的Apply System-Wide应用到全局。然后将当前用户进行注销后再进行登录（注销没有效果，重启就可以了）。

登陆后在右上角出现一个键盘标志，点击进入，选择Configure Current Input Method

![Snipaste_2020-04-02_09-31-25](Snipaste_2020-04-02_09-31-25.png)

进入下面的Input Method界面后，选择+号

![Snipaste_2020-04-02_09-31-31](Snipaste_2020-04-02_09-31-31.png)

进入到Add input method界面，将下面的Only Show Current Language 点掉后，在搜索栏搜索搜狗拼音，选中之后进行添加。

![Snipaste_2020-04-02_09-31-37](Snipaste_2020-04-02_09-31-37.png)

添加成功后，将搜狗拼音移到第一位。

成功之后，打开浏览器随便输入，可以看到输入结果，同时成功后下方还会出现搜狗输入法的标志，这时候就可以通过shirt键切换中英文。

**其他**

不习惯默认中文的小伙伴可以,把sogou放在第二个



## vscode

**安装**

1. 首先，更新包的索引和安装的依赖键入:

   ```
   sudo apt update
   sudo apt install software-properties-common apt-transport-https wget
   ```

2. 接下来，使用以下[wget命令]导入Microsoft GPG密钥

   ```
   wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
   ```

   并使Visual Studio Coderepository通过键入:

   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
   ```

3. 安装最新版本的Visual Studio代码:

   ```
   sudo apt update
   sudo apt install code
   ```



**使用**

终端输入`code`,或者单击vscode图标



**插件推荐**

1. Chinese (Simplified) Language Pack for Visual Studio Code 就问你中文香不香
2. C/C++ 
3. Python
4. Code Runner 可以跑好多种代码,好评



**配置c**

1. 安装vscode插件---------->c/c++

2. 创建一个c文件,输入一些代码

   ```
   include<stdio.h>
   int main()
   {
   printf("hello world");
   }
   ```

3. 终端输入`sudo apt install gcc`

4. 在菜单栏里面选择Terminal-->Configure Tasks-->gcc，会自动帮你生成`.vscode`目录和一个`launch.json`文件

5. 打开c文件按F5,会报错,顺着点就会自动生成第二个文件`tasks.json`

6. 再次按F5就成功编译了



- 我看网上很多对于配置文件有所改动,但是就算博主可以用,也是针对他自己的电脑可以用
- 直接复制粘贴,对路径啊什么的略作修改啊,成功率挺低的
- 所以我想看看直接默认配置会怎么样,毕竟软件做出来是给人用的,搞这么麻烦,不人道啊
- ......可以用了: )
- 比起win10,ubuntu意外的好用呢><



**配置py**

1. 安装python插件
2. 新建test.py文件,会弹出

![Snipaste_2020-04-01_09-32-47](Snipaste_2020-04-01_09-32-47.png)

3. 直接点击安装，由于缺少pip环境会导致安装失败。

4. 于是先安装pip：sudo apt-get install python-pip

5. 然后直接安装即可：pip install pylint



## sublime text3

1. 运行命令添加密钥环：

   ```
   wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
   ```

2. 添加apt存储库:

   ```
   echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
   ```

3. 通过Synaptic包管理器或运行命令安装sublime-text包：

   ```
   sudo apt update && sudo apt install sublime-text
   ```




## java jdk-8

更新软件包列表：

```bash
sudo apt-get update
```

2、安装openjdk-8-jdk：

```bash
sudo apt-get install openjdk-8-jdk
```

3、查看java版本，看看是否安装成功：

```bash
java -version
```



## 调整终端字体大小

默认的实在太小了,眼都快瞎了><

1. 终端右键`Preferences`

2. 按图搞一波~~

   ![Snipaste_2020-04-02_09-37-01](Snipaste_2020-04-02_09-37-01.png)



## pycharm

本来对pycharm无感的来着,在成功申请jetbrains的学生优惠后......

真香

jetbrain的操作都差不多,webstore的安装,修改,加速都和pycharm一样

### 安装

1. 下载linux版本的jetbrains toolbox: https://www.jetbrains.com/toolbox-app/

2. 拖到虚拟机,提取,点击即可使用

3. 十分方便下载,如图

   ![Snipaste_2020-04-02_09-42-08](Snipaste_2020-04-02_09-42-08.png)

   

### 修改字体大小

- file--->setting

- 如图修改菜单栏字体大小

  ![Snipaste_2020-04-02_10-40-17](Snipaste_2020-04-02_10-40-17.png)

- 如图修改代码字体大小

  ![Snipaste_2020-04-02_10-42-32](Snipaste_2020-04-02_10-42-32.png)



### jupyter notebook

**安装**

一开始是从anaconda开始的,所以对jupyter有特殊的情感呢

所以听说pycharm支持了,就搞起来试试

1. 终端:`pip install jupyter`

   ==我看博客上都是直接pip的,但是安装后发现里面只有python2,所以"pip3 install jupyter"试试,ojbk==

2. 下载的过程中报了好几次错(记录一下报错及处理方法)

   - timeout: 继续下,不要管,反正好像是接着下的(我怀疑是网的原因)
   - setuptool: `pip install setuptools==33.1.1`

**使用**

1. 如果不是pycharm用的话,直接终端输入`jupyter notebook`就可以在浏览器中开一个了,直接用就行

   还是原来的配方,还是原来的味道

2. pycharm有两种使用方式

   ==不知道有没有编程浏览器那种的排版方式,左右的真的不习惯啊==

   - new一个file,选择jupyter类型,第一次使用,会提示你下载插件,下载完就行了

     ![Snipaste_2020-04-02_10-01-58](Snipaste_2020-04-02_10-01-58.png)

   - 在终端中使用

     ![Snipaste_2020-04-02_10-02-16](Snipaste_2020-04-02_10-02-16.png)



### 加速

pycharm打开有点慢,而且经常卡死,所以要修改一下配置

调整一下堆的大小,如图操作

1. help-->find action

![Snipaste_2020-04-02_10-17-28](Snipaste_2020-04-02_10-17-28.png)

2. 搜索vm options...

![Snipaste_2020-04-02_10-18-13](Snipaste_2020-04-02_10-18-13.png)

3. 修改

![Snipaste_2020-04-02_10-20-28](Snipaste_2020-04-02_10-20-28.png)

4. 使用体验: 确实快了很多



## 时间同步

搞了半天,一看时间,嗯,还早

退出来一看......wtf!!!

啊,同步时间,迫在眉睫

1. 查看时区(我的已经改过来了,原来是utc)

   - UTC:协调世界时
   - CST:北京时间

   ```
   sudo date
   ```

![Snipaste_2020-04-02_13-01-43](Snipaste_2020-04-02_13-01-43.png)

2. 查看是否同步

```
sudo timedatectl
```

![Snipaste_2020-04-02_13-05-13](Snipaste_2020-04-02_13-05-13.png)

- 如果是no

  重启服务(看看yes了没)

  ```
  sudo systemctl restart systemd-timesyncd.service
  ```

  启动服务(再康康yes了没)

  ```
  sudo timedatectl set-ntp true
  ```

  还不行?那我也木的办法了

  

3. 更改时区

   - 列出可用时区(按q退出)

     ```
     timedatectl list-timezones
     ```

     

     ![Snipaste_2020-04-02_13-10-19](Snipaste_2020-04-02_13-10-19.png)

   - 设置新时区(以上海为例)

     ```
     sudo timedatectl set-timezone Asia/Shanghai
     ```

   

4. 要是想换回原来的时区

   ```
   sudo timedatectl set-timezone UTC
   ```




## 搭服务器

先下一堆东西

1. ssh

   ```
   sudo apt-get install ssh
   ```

2. apache

   ```
   sudo apt-get install apache2
   ```

   apache默认网站的文件根目录在/var/www下面，html文件夹下面有个index.html里面记录的信息就是我们当时访问localhost，浏览器所显示的东西

   - sudo systemctl start apache2 //启动

   - sudo systemctl stop apache2 //停止

   - sudo systemctl restart apache2 //重启

   - sudo systemctl reload apache2 //在不重新启动连接的情况下应用配置更改。

   - sudo systemctl disable apache2 //禁止开机自启

3. mysql

   ```
   sudo apt-get install mysql-server
   ```

4. mysql-client

   ```
   sudo apt-get install mysql-client
   ```

5. php

   ```
   sudo apt-get install php
   ```

6. php&apache的连接件

   ```
   sudo apt-get install libapache2-mod-php
   ```

7. phpmyadmin

   ```
   sudo apt-get install phpmyadmin
   ```





## 中文

1. 在应用中找到`language supporting`

![Snipaste_2020-04-03_11-09-13](Snipaste_2020-04-03_11-09-13.png)

2. `install/remove language`-->`chines(simplifled)`-->`apply`

![Snipaste_2020-04-03_11-10-12](Snipaste_2020-04-03_11-10-12.png)

3. 下载过程中会让你输入密码

4. 手动把中国汉字拉到第一个,apply

   ![Snipaste_2020-04-03_11-17-26](Snipaste_2020-04-03_11-17-26.png)

5. 重启

6. ojbk



## 美化

1. `sudo apt-get install gnome-tweak-tool`

2. `sudo apt-get install  gnome-shell-extension-dashtodock`

   扩展,用于配置任务栏(我想把放到下面并居中)

3. 重启

4. `gnome-tweaks`,或从应用中打开,中文名为优化

   ![Snipaste_2020-04-03_11-44-19](Snipaste_2020-04-03_11-44-19.png)

5. 根据自己的喜好进行配置

附我的配置

1. 主题(其实这个外观已经时应用后的样子了)

   ![Snipaste_2020-04-03_11-45-40](Snipaste_2020-04-03_11-45-40.png)

2. 任务栏

   ![Snipaste_2020-04-03_11-47-58](Snipaste_2020-04-03_11-47-58.png)

   配置

   ![Snipaste_2020-04-03_11-50-04](Snipaste_2020-04-03_11-50-04.png)

   结果

   ![Snipaste_2020-04-03_11-51-51](Snipaste_2020-04-03_11-51-51.png)





## 腾点空间

为啥突然想不开,搞清理呢?

我的破电脑空间不够用了,大清理了一番

居安思危,觉得虚拟机也得清理  ~~(什么歪理啊,tui)~~



### 清理垃圾

Ubuntu系统在运行时是不会产生无用垃圾的。但是我们在升级系统时，软件管理器下载的软件包，系统则不会自动删除，其实这样做也是考虑到你可能会再次安装从而加快再次安装的速度考虑。

删除已卸载掉软件包

`sudo apt-get autoclean`

删除所有安装包

`sudo apt-get clean`

删除孤立包(某些软件的依赖项,但别的软件用不上)

`sudo apt-get autoremove`



### 删除老旧内核

1. 查看内核版本,免得误删

   ```
   uname -r
   ```

   ![image-20200404123816670](image-20200404123816670.png)

2. 显示文件

   ```
   dpkg --get-selections | grep linux
   ```

   已经删完了,就补张图好了><

   ![image-20200404124053791](image-20200404124053791.png)

3. 删掉低版本的内核文件image、头文件headers

   ```
   sudo apt-get purge  内核文件名  头文件名
   ```





## git

1. 下载(我下的时候已经默认存在了)

   ```
   sudo apt install git
   ```

2. 查看版本

   ```
   git --version
   ```

3. 配置名称和邮箱

   ```
   git config --global user.name "123"
   git config --global user.email "123@123.net"
   ```

   可通过查看.gitconfig来验证配置更改

   ```
   git config --list
   ```

   如果用了 **--global** 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。

   如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。



## 问题

N: 无法安全地用该源进行更新，所以默认禁用该源。

![Snipaste_2020-04-04_13-01-51](Snipaste_2020-04-04_13-01-51.png)

打开**软件与更新**

![Snipaste_2020-04-04_13-02-13](Snipaste_2020-04-04_13-02-13.png)

**其他软件**-->把勾去掉

![Snipaste_2020-04-04_13-03-15](Snipaste_2020-04-04_13-03-15.png)



## 参考

> 奇奇怪怪: https://zhuanlan.zhihu.com/p/56253982
>
> sogou: https://blog.csdn.net/lupengCSDN/article/details/80279177
>
> vscode
>
> >vscode安装: https://linuxize.com/post/how-to-install-visual-studio-code-on-ubuntu-18-04/
> >
> >配置py: https://blog.csdn.net/yk150915/article/details/81087282
> >
> >配置c: https://segmentfault.com/a/1190000020155987
>
> sublime text3: https://www.linuxidc.com/Linux/2019-03/157533.htm
>
> jdk-8: https://blog.csdn.net/zbj18314469395/article/details/86064849
>
> pycharm
>
> > jupyter安装失败: https://blog.csdn.net/jiangzubing520/article/details/80253792?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
> >
> > pycharm中jupyter使用方法: https://blog.csdn.net/xiemanR/article/details/71837385
> >
> > pycharm加速: https://blog.csdn.net/jack339083590/article/details/79261717?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
>
> 时间同步: https://linux.cn/article-11220-1.html
>
> 中文: https://m.jb51.net/os/Ubuntu/298601.html
>
> 美化: https://blog.csdn.net/weixin_43629813/article/details/100525856
>
> 清理: https://blog.csdn.net/levon2018/article/details/81746613?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1
>
> git: https://www.linuxidc.com/Linux/2018-05/152610.htm
>
> 问题: https://blog.csdn.net/weixin_42966187/article/details/89380505





