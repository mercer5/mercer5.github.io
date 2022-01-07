---
title: padding oracle attack
date: 2022-01-06 16:11:13
categories: 密码学
tag:
- 密码学
- 算法
- 安全
mathjax: true
---

# padding oracle attack


> [详解参考](https://www.dazhuanlan.com/aladengodman/topics/1812953)
>
> [学长blog](https://www.tr0y.wang/2017/10/06/Crypto1/)

## 问题描述

In this assignment, you must decrypt a challenge ciphertext generated using AES in CBC-mode with PKCS #5 padding. (Note: technically this is PKCS #7 padding,since the block size of AES is 16 bytes. But the padding is done in exactly the same way as PKCS #5 padding.) To do so, you will be given access to a server that will decrypt any ciphertexts you send it (using the same key that was used to generate the challenge ciphertext)…but that will only tell you whether or not decryption results in an error!

要求解密一个 CBC 工作模式，PKCS #7 方式填充，AES 加密的挑战密文。为此，你需要访问一个服务器（该服务器能用挑战密钥解密任何你发送的密文），它将返回你发送的密文是否填充正确。

All the files needed for this assignment are available here, including a README file that should explain everything.

此作业所需的所有文件都已[在这里](http://oqx4hhfj3.bkt.clouddn.com/PA2-AES.rar)给出，包括一个解释一切的 README 文件。

Note that this assignment requires the ability to perform basic networking. Because we do not assume students necessarily know this, we have provided stub code for doing basic networking in C, Java, Ruby, and Python, but you are welcome to use any language of your choice as long as you are able to write code for basic networking functionality in that language. (Students may feel free to post stub code in other languages for the networking component on the discussion boards.)

注意到此作业需要执行基本网络的功能。因为我们不要求学生必须知道这一点，所以我们提供了在 C，Java，Ruby 和 Python 中进行基本网络连接的存根代码，但是欢迎使用任何您选择的语言，只要您能够编写代码该语言的基本网络功能。（学生可以自由地在讨论板上为其他语言的网络组件发布存根代码。）

The first step in this project is to send the challenge ciphertext to the server, and verify that you receive back a “no error” message. Once you can do that, the rest is “just” crypto…

该项目的第一步是将挑战密文发送到服务器，并验证您是否收到 “无错误” 消息。一旦你可以做到这一点，其余的是 “只是” 加密…

The plaintext,when converted to ASCII, is readable English text, and so you should be able to tell once you have been successful. Once you have successfully recovered the plaintext (in ASCII).

明文转换为 ASCII 时，是可读的英文文本，所以一旦成功恢复明文，便可得知。



## 前置技能

### CBC 工作模式

CBC 模式是一种分组链接模式，目的是为了使原本独立的分组密码加密过程形成迭代，使每次加密的结果影响到下一次加密。初始化向量 IV 与第一组明文(分组 1)异或后，再经过运算得到的结果作为新的 IV，用于下一分组(分组 2)，不断迭代下去：
[![加密](20180917032437175.png)](https://rzx1szyykpugqc-1252075454.piccd.myqcloud.com/Crypto1/20180917032437175.png!blog)



解密过程是加密过程的逆过程：
[![解密](20180917032510248.png)](https://rzx1szyykpugqc-1252075454.piccd.myqcloud.com/Crypto1/20180917032510248.png!blog)



### PKCS #7 填充

以整字节填充。每个填充字节的值是用于填充的字节数，即是说，添加 N 个字节，每个的值都是 N。 填充的字节数取决于信息末尾到块边缘的距离。

如:

> 分组长度8bytes
>
> dd: 数据

缺少 1 位(1 个 Padding)， 填充位为： `0x01`

```
dd dd dd dd dd dd dd 01
```

缺少 5 位(5 个 Padding)， 填充位为： `0x05 0x05 0x05 0x05 0x05`

```
dd dd dd 05 05 05 05 05
```



### padding-oracle attack 攻击原理

padding-oracle attack 针对 CBC 工作模式攻击，与具体加密算法无关。

服务器判断解密出的明文是否出错一般从后开始检验明文末尾字节的值，若末尾为`0x01` 则末尾一字节为`0x01`，填充末尾为`0x03` 则末尾三字节均为`0x03` 。利用服务器的返回值，可以在不知道密钥的情况下得到明文。



#### 假设

例如现在有一个 CBC 工作模式，PKCS #7 方式填充的密文 IV||C（分组大小为 8 字节）：

```
21 A2 DD 04 35 0A BA 58 | CD DD AA DD 34 23 A1 64 |
```

为了方便描述，用 $C[i]$ 表示第 $i$ 个分组，$C[i][j]$ 表示第 $i$ 个分组第 $j$ 个字节，$D(*)$ 表示解密算法。

我们想要解密最后一个分组 $C[n]$ ，即求 $P[n]$ 。

先求明文最后一个分组的最后一位，即 $P[n][6]$ 。



#### 最后一位

假设此时仅填充一个字节，构造初始向量 $IV_0$ 值为：

```
| 00 00 00 00 00 00 00 00 |
```

向服务器发送这两个分组构成的密文 $IV_0||C[n]$ ：

```
| 00 00 00 00 00 00 00 00 | CD DD AA DD 34 23 A1 64 |
```

要使服务器的返回值为 $1$（即显示 Padding 正确），那么明文最后一位应该为 `0x01` 。

那么我们需要遍历 $[0,256)$ 修改 $IV_0[7]$ 的值，直到服务器的返回值为 $1$ 。

假设此时初始向量 $IV_1$ 为：

```
| 00 00 00 00 00 00 00 A0 |
```

此时, 明文末尾一位为 $(D(C[n]) \oplus IV_1)[7] == 0x01$。

于是可以得到
$$
D(C[n])[7] = 0x01 \oplus IV_1[7] \\
P[n][7] = D(C[n])[7] \oplus C[n-1][7] = 0x01 \oplus IV_1[7] \oplus C[n-1][7]\\
$$
至此，明文最后一个分组的最后一位已经得到，接下来求明文最后一个分组的倒数第二位。



#### 倒数第二位

**1.修正最后一位**

> 目的: 使明文的最后一位是 `0x02` 
>
> 方法: 在已知a, b, 且 a xor b = 1 的情况下, 容易获得a' s.t. a' xor b = 2

此时假设填充了两个字节，即明文末尾两位均为 $0x02$ 。

由于 $D(C[n][7]) = 0x01 \oplus IV_1[7]$ ，所以当 $D(C[n][7]) \oplus IV_1^{'}[7] = 0x02$ 时，有
$$
IV_1^{'}[7] = D(C[n][7]) \oplus 0X02 = 0X01 \oplus IV_1[7] \oplus 0X02\\
$$
此时的初始向量 $IV_1^{'}$ 为：

```
| 00 00 00 00 00 00 00 A3 |
```

**2.获取倒数第二位**

> 目的: 使明文的倒数第二位是 `0x02` , 以满足padding的要求

遍历 $[0,256)$ 修改 $IV_1^{'}[6]$ 的值，直到服务器的返回值为 $1$ 。

假设此时初始向量 $IV_2$ 为：

```
| 00 00 00 00 00 00 40 A3 |
```

由于, 明文倒数第二位为`0x02`:  $D(C[n][6]) \oplus IV_2[6] = 0x02$ 

于是可得: 
$$
D(C[n][6]) = 0X02 \oplus IV_2[6] \\
P[n][6] = D(C[n][6]) \oplus C[n-1][6] = 0X02 \oplus IV_2[6] \oplus C[n-1][6]\\
$$
明文最后一个分组的倒数第二位也已经得到。

依次类推，由此即可以仅根据密文还原出明文。



#### 一般情况

> 第i轮: 从1开始,注意写代码时数组从0开始

**1.准备**

开3个数组

- iv = [全0(数字)]
- Ivalue= []
- m = []

**2.修正**

$IV_{i-1}->IV_{i-1}^{'}$:  $Ivalue$ 中的所有**非零值**与 $0xi$ 进行异或

**3.遍历**

获取$IV_{i}$

- iv[-i] 赋值

**4.计算数据,并保存**
$$
Ivalue[-i] = D(C[n][-i]) = 0xi \oplus IV_i[-i] \\
P[n][-i] = D(C[n][-i]) \oplus C[n-1][-i]
$$

- Ivalue.append()
- m.append()



## exp

### 模拟

> 由于没连接上服务器,所以用随机数来表示是否能正确解密XD
>
> 之后需修改judge函数

```python
import random
random.seed(10)
c = ['9F0B13944841A832B2421B9EAF6D9836',
    '813EC9D944A5C8347A7CA69AA34D8DC0',
    'DF70E343C4000A2AE35874CE75E64C31']
# 判断返回是否正确
def judge(build):
    return random.randint(0,100)

# 从最后一块开始破译 c[-x]
for x in range(1,len(c)):
    print("Detecting Block {}".format(x))
    block = c[-x] # 当前分组
    prior_block = [int(i) for i in bytes.fromhex(c[-x-1])] # 前一分组
    
    iv = [0]*16 # 初始向量
    Ivalue = [] # 中间值
    m = [] # 明文(hex)
    
    # 一个分组16B
    for i in range(1,17):
        print(" round {}".format(i))
        
        # 修正
        l = len(Ivalue)
        iv = iv[:16-l] + [x^i for x in Ivalue[::-1]]

        # 爆破vi[-i]
        for j in range(pow(2,8)):
            iv[-i] = j
            build = ''.join([str(hex(i)[2:].zfill(2)) for i in iv]) + block
            if judge(build):
                break
        
        # 更新
        Ivalue.append(iv[-i]^i)
        m.append(Ivalue[-1]^prior_block[-i])
        print(" - Ivalue : {}".format(''.join([str(hex(i)[2:].zfill(2)) for i in Ivalue[::-1]])))
        print(" - m      : {}".format(''.join([str(hex(i)[2:].zfill(2)) for i in m[::-1]])))
```

```
Detecting Block 1
 round 1
 - Ivalue : 01
 - m      : c1
 round 2
 - Ivalue : 0201
 - m      : 8fc1
 round 3
 - Ivalue : 030201
 - m      : 4e8fc1
 round 4
 - Ivalue : 04030201
 - m      : a74e8fc1
 round 5
 - Ivalue : 0504030201
 - m      : 9fa74e8fc1
 round 6
 - Ivalue : 060504030201
 - m      : a09fa74e8fc1
 round 7
 - Ivalue : 07060504030201
 - m      : 7ba09fa74e8fc1
 round 8
 - Ivalue : 0807060504030201
 - m      : 727ba09fa74e8fc1
 round 9
 - Ivalue : 090807060504030201
 - m      : 3d727ba09fa74e8fc1
 round 10
 - Ivalue : 0a090807060504030201
 - m      : c23d727ba09fa74e8fc1
 round 11
 - Ivalue : 0b0a090807060504030201
 - m      : aec23d727ba09fa74e8fc1
 round 12
 - Ivalue : 0c0b0a090807060504030201
 - m      : 48aec23d727ba09fa74e8fc1
 round 13
 - Ivalue : 0d0c0b0a090807060504030201
 - m      : d448aec23d727ba09fa74e8fc1
 round 14
 - Ivalue : 0e0d0c0b0a090807060504030201
 - m      : c7d448aec23d727ba09fa74e8fc1
 round 15
 - Ivalue : 0f0e0d0c0b0a090807060504030201
 - m      : 31c7d448aec23d727ba09fa74e8fc1
 round 16
 - Ivalue : 100f0e0d0c0b0a090807060504030201
 - m      : 9131c7d448aec23d727ba09fa74e8fc1
Detecting Block 2
 round 1
 - Ivalue : 01
 - m      : 37
 round 2
 - Ivalue : 0201
 - m      : 9a37
 round 3
 - Ivalue : 030201
 - m      : 6e9a37
 round 4
 - Ivalue : 04030201
 - m      : ab6e9a37
 round 5
 - Ivalue : 0504030201
 - m      : 9bab6e9a37
 round 6
 - Ivalue : 060504030201
 - m      : 1d9bab6e9a37
 round 7
 - Ivalue : 07060504030201
 - m      : 451d9bab6e9a37
 round 8
 - Ivalue : 0807060504030201
 - m      : ba451d9bab6e9a37
 round 9
 - Ivalue : 090807060504030201
 - m      : 3bba451d9bab6e9a37
 round 10
 - Ivalue : 0a090807060504030201
 - m      : a23bba451d9bab6e9a37
 round 11
 - Ivalue : 0b0a090807060504030201
 - m      : 4aa23bba451d9bab6e9a37
 round 12
 - Ivalue : 0c0b0a090807060504030201
 - m      : 444aa23bba451d9bab6e9a37
 round 13
 - Ivalue : 0d0c0b0a090807060504030201
 - m      : 99444aa23bba451d9bab6e9a37
 round 14
 - Ivalue : 0e0d0c0b0a090807060504030201
 - m      : 1d99444aa23bba451d9bab6e9a37
 round 15
 - Ivalue : 0f0e0d0c0b0a090807060504030201
 - m      : 041d99444aa23bba451d9bab6e9a37
 round 16
 - Ivalue : 100f0e0d0c0b0a090807060504030201
 - m      : 8f041d99444aa23bba451d9bab6e9a37
```



### 确认服务器可用

```python
from pwn import *
r = remote('128.8.130.16', 49101)
msg= "9F0B13944841A832B2421B9EAF6D9836813EC9D944A5C8347A7CA69AA34D8DC0DF70E343C4000A2AE35874CE75E64C31"
msg = bytes.fromhex(msg)
lst = list(msg)
# 前面添加分组数目(包括IV)
lst.insert(0,3)
# 最后添加一个0
lst.append(0)
r.send(bytes(lst))
print(r.recv(numb=2).decode()[0])
```

最后若返回`1`则说明服务器正常

### 交互

```python
from pwn import *
c = ['9F0B13944841A832B2421B9EAF6D9836',
    '813EC9D944A5C8347A7CA69AA34D8DC0',
    'DF70E343C4000A2AE35874CE75E64C31']
r = remote('128.8.130.16',49101)
# f判断返回是否正确
def send_payload(build):
    msg = bytes.fromhex(build)
    lst = list(msg)
    lst.insert(0,2)
    lst.append(0)
    r.send(bytes(lst))
    return r.recv(numb=2).decode()[0] == '1'


# 从最后一块开始破译 c[-x]
for x in range(1,len(c)):
    print("Detecting Block {}".format(x))
    block = c[-x] # 当前分组
    prior_block = [int(i) for i in bytes.fromhex(c[-x-1])] # 前一分组
    
    iv = [0]*16 # 初始向量
    Ivalue = [] # 中间值
    m = [] # 明文(hex)
    
    # 一个分组16B
    for i in range(1,17):
        print(" round {}".format(i))
        
        # 修正
        l = len(Ivalue)
        iv = iv[:16-l] + [x^i for x in Ivalue[::-1]]

        # 爆破vi[-i]
        for j in range(1,pow(2,8)):
            iv[-i] = j
            build = ''.join([str(hex(i)[2:].zfill(2)) for i in iv]) + block
            if send_payload(build):
                break
        
        # 更新
        Ivalue.append(iv[-i]^i)
        m.append(Ivalue[-1]^prior_block[-i])
        print(" - IV     : {}".format(''.join([str(hex(i)[2:].zfill(2)) for i in iv])))
        print(" - Ivalue : {}".format(''.join([str(hex(i)[2:].zfill(2)) for i in Ivalue[::-1]])))
        print(" - m      : {}".format(''.join([str(hex(i)[2:].zfill(2)) for i in m[::-1]])))
    
    # print the message
    print(''.join(chr(i) for i in m[::-1]))
```

```
Detecting Block 1
 round 1
 - IV     : 000000000000000000000000000000ca
 - Ivalue : cb
 - m      : 0b
 round 2
 - IV     : 000000000000000000000000000084c9
 - Ivalue : 86cb
 - m      : 0b0b
 round 3
 - IV     : 000000000000000000000000004585c8
 - Ivalue : 4686cb
 - m      : 0b0b0b
 round 4
 - IV     : 000000000000000000000000ac4282cf
 - Ivalue : a84686cb
 - m      : 0b0b0b0b
 round 5
 - IV     : 000000000000000000000094ad4383ce
 - Ivalue : 91a84686cb
 - m      : 0b0b0b0b0b
 round 6
 - IV     : 00000000000000000000ab97ae4080cd
 - Ivalue : ad91a84686cb
 - m      : 0b0b0b0b0b0b
 round 7
 - IV     : 00000000000000000070aa96af4181cc
 - Ivalue : 77ad91a84686cb
 - m      : 0b0b0b0b0b0b0b
 round 8
 - IV     : 0000000000000000797fa599a04e8ec3
 - Ivalue : 7177ad91a84686cb
 - m      : 0b0b0b0b0b0b0b0b
 round 9
 - IV     : 0000000000000036787ea498a14f8fc2
 - Ivalue : 3f7177ad91a84686cb
 - m      : 0b0b0b0b0b0b0b0b0b
 round 10
 - IV     : 000000000000c9357b7da79ba24c8cc1
 - Ivalue : c33f7177ad91a84686cb
 - m      : 0b0b0b0b0b0b0b0b0b0b
 round 11
 - IV     : 0000000000a5c8347a7ca69aa34d8dc0
 - Ivalue : aec33f7177ad91a84686cb
 - m      : 0b0b0b0b0b0b0b0b0b0b0b
 round 12
 - IV     : 0000000061a2cf337d7ba19da44a8ac7
 - Ivalue : 6daec33f7177ad91a84686cb
 - m      : 290b0b0b0b0b0b0b0b0b0b0b
 round 13
 - IV     : 000000e960a3ce327c7aa09ca54b8bc6
 - Ivalue : e46daec33f7177ad91a84686cb
 - m      : 3d290b0b0b0b0b0b0b0b0b0b0b
 round 14
 - IV     : 0000e7ea63a0cd317f79a39fa64888c5
 - Ivalue : e9e46daec33f7177ad91a84686cb
 - m      : 203d290b0b0b0b0b0b0b0b0b0b0b
 round 15
 - IV     : 001fe6eb62a1cc307e78a29ea74989c4
 - Ivalue : 10e9e46daec33f7177ad91a84686cb
 - m      : 2e203d290b0b0b0b0b0b0b0b0b0b0b
 round 16
 - IV     : d000f9f47dbed32f6167bd81b85696db
 - Ivalue : c010e9e46daec33f7177ad91a84686cb
 - m      : 412e203d290b0b0b0b0b0b0b0b0b0b0b
A. =)\x0b\x0b\x0b\x0b\x0b\x0b
Detecting Block 2
 round 1
 - IV     : 00000000000000000000000000000017
 - Ivalue : 16
 - m      : 20
 round 2
 - IV     : 0000000000000000000000000000f414
 - Ivalue : f616
 - m      : 6e20
 round 3
 - IV     : 000000000000000000000000000ff515
 - Ivalue : 0cf616
 - m      : 616e20
 round 4
 - IV     : 0000000000000000000000008b08f212
 - Ivalue : 8f0cf616
 - m      : 20616e20
 round 5
 - IV     : 0000000000000000000000ef8a09f313
 - Ivalue : ea8f0cf616
 - m      : 7420616e20
 round 6
 - IV     : 0000000000000000000078ec890af010
 - Ivalue : 7eea8f0cf616
 - m      : 657420616e20
 round 7
 - IV     : 0000000000000000002279ed880bf111
 - Ivalue : 257eea8f0cf616
 - m      : 67657420616e20
 round 8
 - IV     : 00000000000000009a2d76e28704fe1e
 - Ivalue : 92257eea8f0cf616
 - m      : 2067657420616e20
 round 9
 - IV     : 000000000000004e9b2c77e38605ff1f
 - Ivalue : 4792257eea8f0cf616
 - m      : 752067657420616e20
 round 10
 - IV     : 000000000000cd4d982f74e08506fc1c
 - Ivalue : c74792257eea8f0cf616
 - m      : 6f752067657420616e20
 round 11
 - IV     : 000000000013cc4c992e75e18407fd1d
 - Ivalue : 18c74792257eea8f0cf616
 - m      : 596f752067657420616e20
 round 12
 - IV     : 000000006414cb4b9e2972e68300fa1a
 - Ivalue : 6818c74792257eea8f0cf616
 - m      : 20596f752067657420616e20
 round 13
 - IV     : 000000b86515ca4a9f2873e78201fb1b
 - Ivalue : b56818c74792257eea8f0cf616
 - m      : 2120596f752067657420616e20
 round 14
 - IV     : 000064bb6616c9499c2b70e48102f818
 - Ivalue : 6ab56818c74792257eea8f0cf616
 - m      : 792120596f752067657420616e20
 round 15
 - IV     : 006565ba6717c8489d2a71e58003f919
 - Ivalue : 6a6ab56818c74792257eea8f0cf616
 - m      : 61792120596f752067657420616e20
 round 16
 - IV     : d67a7aa57808d75782356efa9f1ce606
 - Ivalue : c66a6ab56818c74792257eea8f0cf616
 - m      : 5961792120596f752067657420616e20
Yay! You get an 
```

```
Yay! You get an A. =)
```

