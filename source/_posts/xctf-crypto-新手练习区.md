---
title: xctf-crypto-新手练习区
date: 2020-08-25 08:37:43
categories: ctf
tags: 
- ctf
- secure
---


## 1. base64

在线解密,或是自己写脚本都行



## 2. Caesar

同上



## 3. Morse

同上



## 4. 混合编码

1. base64
2. html实体编码
3. base64
4. ascii



## 5. 不仅仅是Morse

1. morese解密

   得到一串奇奇怪怪的东西

   ```
   MAY_BE_HAVE_ANOTHER_DECODEHHHHAAAAABAABBBAABBAAAAAAAABAABABAAAAAAABBABAAABBAAABBAABAAAABABAABAAABBABAAABAAABAABABBAABBBABAAABABABBAAABBABAAABAABAABAAAABBABBAABBAABAABAAABAABAABAABABAABBABAAAABBABAABBA
   ```

2. 后面的AB的长度为170

   - 5的倍数
   - 只有两个字母
   - 2^5^为32,可以很方便的表示26个字母
   - 所以假设0-25,分别对应a-z

3. 尝试解密看看

   ```python
   #分割替换
   s="AAAAABAABBBAABBAAAAAAAABAABABAAAAAAABBABAAABBAAABBAABAAAABABAABAAABBABAAABAAABAABABBAABBBABAAABABABBAAABBABAAABAABAABAAAABBABBAABBAABAABAAABAABAABAABABAABBABAAAABBABAABBA"
   s=s.replace("A","0")
   s=s.replace("B","1")
   lst=[]
   while(s):
       lst.append(s[:5])
       s=s[5:]
   print(lst)
   #生成字典
   import string
   b=string.ascii_lowercase
   l1=[]
   l2=[]
   for i in b:
       l1.append(i)
   for i in range(26):
       l2.append(i)
   dic=dict(zip(l2,l1))
   print(dic)
   #flag
   for i in lst:
       num=int(i,2)
       print(dic.get(num),end="")
   ```

   

   ## 6. 幂数加密

   ```python
   #字母数字对应表生成
   import string
   l1=[]
   l2=[]
   for i in range(1,27):
       l1.append(i)
   for i in string.ascii_uppercase:
       l2.append(i)
   dic=dict(zip(l1,l2))
   print(dic)
   #字符串处理
   a="8842101220480224404014224202480122"
   lst=a.split("0")
   lst=[eval(x) for x in lst]
   lst1=[]
   for i in lst:
       sum=0
       while(i):
           sum+=i%10
           i=i//10
       lst1.append(sum)
   print(lst1)
   #flag
   flag=""
   for i in lst1:
       flag+=dic[i]
   print(flag)
   ```



## 6. easychallenge

给的是一个pyc文件,我就在在线python反编译网站上反编译了一下

得到python原码

```python
import base64
def encode1(ans):
    s = ''
    for i in ans:
        x = ord(i) ^ 36
        x = x + 25
        s += chr(x)

    return s

def encode2(ans):
    s = ''
    for i in ans:
        x = ord(i) + 36
        x = x ^ 36
        s += chr(x)

    return s

def encode3(ans):
    return base64.b32encode(ans)


flag = ' '
print('Please Input your flag:')
flag = input()
final = 'UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E==='
if encode3(encode2(encode1(flag))) == final:
    print('correct')
else:
    print('wrong')
```

emmm,然后就好办了,加密方法都在上面了

```python
import base64
s="UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E==="
s1=base64.b32decode(s)
print(s1)

s2=""
for i in s1:
    x=i^36
    x-=36
    s2+=chr(x)
print(s2)

s3=""
for i in s2:
    x=ord(i)-25
    x=x^36
    s3+=chr(x)
print(s3)
```



## 7. Normal_RSA

打开附件,里面是`pubkey.pem`和`flag.enc`

我们可以利用kali里的openssl来搞

1. 把文件拖虚拟机里

2. 终端输入openssl

3. `openssl rsa -pubin -text -modulus -in warmup -in pubkey.pem`

   - exponent:E
   - modulus:N

   以上两个要用复制好

4. 上网找在线分解质因数的搞出P,Q

5. 然后在kali中使用(代码如下,pqe自填),会生成一个private.pem

   ```python
   #coding=utf-8
   import math
   import sys
   from Crypto.PublicKey import RSA
   arsa=RSA.generate(1024)
   arsa.p=
   arsa.q=
   arsa.e=
   arsa.n=arsa.p*arsa.q
   Fn=long((arsa.p-1)*(arsa.q-1))
   i=1
   while(True):
       x=(Fn*i)+1
       if(x%arsa.e==0):
              arsa.d=x/arsa.e
              break
       i=i+1
   private=open('private.pem','w')
   private.write(arsa.exportKey())
   private.close()
   ```

6. 在终端中打开openssl,输入`rsautl -decrypt -in flag.enc -inkey private.pem`

7. ojbk



## 8. 转轮机加密

题目:

```
1:  < ZWAXJGDLUBVIQHKYPNTCRMOSFE <
2:  < KPBELNACZDTRXMJQOYHGVSFUWI <
3:  < BDMAIZVRNSJUWFHTEQGYXPLOCK <
4:  < RPLNDVHGFCUKTEBSXQYIZMJWAO <
5:  < IHFRLABEUOTSGJVDKCPMNZQWXY <
6:  < AMKGHIWPNYCJBFZDRUSLOQXVET <
7:  < GWTHSPYBXIZULVKMRAFDCEONJQ <
8:  < NOZUTWDCVRJLXKISEFAPMYGHBQ <
9:  < XPLTDSRFHENYVUBMCQWAOIKZGJ <
10: < UDNAJFBOWTGVRSCZQKELMXYIHP <
11： < MNBVCXZQWERTPOIUYALSKDJFHG <
12： < LVNCMXZPQOWEIURYTASBKJDFHG <
13： < JZQAWSXCDERFVBGTYHNUMKILOP <

密钥为：2,3,7,5,13,12,9,1,8,10,4,11,6
密文为：NFQKSEVOQOFNP
```

大概就像旧式电话一样

对应着电话号码(2,3,7,5,13,12,9,1,8,10,4,11,6)

把对应轮数中的对应字符(N F Q K S E V O Q O F N P)

拨到第一个去(看下图第一列,与密文相同)

![image-20200404230355691](image-20200404230355691.png)

然后竖着看,直到找到有实际意义的字符串

```python
def circle(string,char):
    while string[0]!=char:
        string=string[1:]+string[0]
    return string

s="""ZWAXJGDLUBVIQHKYPNTCRMOSFE 
KPBELNACZDTRXMJQOYHGVSFUWI 
BDMAIZVRNSJUWFHTEQGYXPLOCK 
RPLNDVHGFCUKTEBSXQYIZMJWAO 
IHFRLABEUOTSGJVDKCPMNZQWXY 
AMKGHIWPNYCJBFZDRUSLOQXVET 
GWTHSPYBXIZULVKMRAFDCEONJQ 
NOZUTWDCVRJLXKISEFAPMYGHBQ 
XPLTDSRFHENYVUBMCQWAOIKZGJ 
UDNAJFBOWTGVRSCZQKELMXYIHP 
MNBVCXZQWERTPOIUYALSKDJFHG 
LVNCMXZPQOWEIURYTASBKJDFHG 
JZQAWSXCDERFVBGTYHNUMKILOP"""
s=s.replace(" \n"," ")
lst=s.split(' ')

key="2,3,7,5,13,12,9,1,8,10,4,11,6"
key=key.split(",")
key=[int(i)-1 for i in key]

x="NFQKSEVOQOFNP"
end=[]
for i in range(len(x)):
    tempstr=lst[key[i]]
    word=x[i]
    end.append(circle(tempstr,word))
for i in range(26):
    for j in range(13):
        print(end[j][i].lower(),end="")
    print("\n")

```

输出如下

```
nfqksevoqofnp
ahgcxiusnwcbn
ctwpcubfotuvy
zetmdrmezgkcc
dqhneyczuvtxj
tgszrtqwtrezb
rypqfawawsbqf
xxywvsaxdcswz
mpbxbbojczxed
jlxygkigvqqrr
qoiitjkdrkytu
oczhydzljeips
ykufhfgullzol
hblrnhjbxmmio
gdvlugxvkxjuq
vmkamlpiiywyx
sambkvlqsiaav
fireinthehole
uzaulcdkfprst
wvfoomsyaupka
irdtpxrppdldm
kncsjzfnmnnjk
psegzphtyadfg
bjojqqecgjvhh
eunvaonrhfhgi
lwjdwwymbbgmw
```

发现就**fireinthehole**是个可读的字符串



## 9. easy_ECC

是的,我不会

