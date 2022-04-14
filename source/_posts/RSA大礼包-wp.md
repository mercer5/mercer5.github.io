---
title: RSA大礼包_wp
date: 2022-01-07 12:47:50
categories: 密码学
tag:
- 密码学
- 算法
- 安全
- XDU
mathjax: true
---

# RSA大礼包_wp

## frame0,4

Common Modulus Attack

**前提条件**

模数一样,待加密的明文一样,公钥互素

- Alice: $(n_1,e_1,m_1)$, Bob: $(n_2,e_2,m_2)$
- $n_1 = n_2$
- $m_1 = m_2$
- $gcd(e_1,e_2) = 1$ 

**攻击原理**
$$
存在 (s,t) \quad s.t.\ e_1s+e_2t=gcd(e_1,e_2)=1 \\\\
c_1^s \cdot c_2^t \equiv (m^{e_1})^s \cdot (m^{e_2})^t \equiv m^{e_1s+e_2t} \equiv m \pmod n
$$

**exp**

```python
from Crypto.Util.number import inverse
from gmpy2 import gcdext

def common_modulus(e1, e2, n, c1, c2):
    # 调用扩展的欧几里得算法函数
    g, s, t = gcdext(e1, e2)
    # 求模反元素
    if s < 0:
        s = -s
        c1 = inverse(c1,n)
    if t < 0:
        t = -t
        c2 = inverse(c2,n)
    return pow(c1,s,n)*pow(c2,t,n)%n
```

**结果**

```
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00My secre'
```



## frame1,18

模不互素攻击

**前提条件**

- $gcd(n1,n2)!=1$
- $n_1 \neq n_2$

**攻击原理**
$$
最大公因数获取其中一个大素数: p = gcd(n1,n2) \\\\
n除p获取另一个大素数: q_i = n_i \div p \quad (i=1 \ or \  2) \\\\
phi_i(n) = (q_i-1)(p-1) \\\\
d_i = inverse(e_i,n_i) \\\\
m_i \equiv c_i^{d_i} \pmod n_i
$$

**结果**

```
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x0b\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00. Imagin'
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\n\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00m A to B'
```



## frame10

Fermat's factorization method

**前置**
$$
\because \forall 奇数n = ab = x^2-y^2 \\\\
\therefore n = ab = (x+y)(x-y) \\\\
$$

$$
             a = x+y \\\\
             b = x-y 
$$

**算法**
$$
y^2 = x^2 - n >= 0 \\\\
\therefore x^2>=n ,即: x>=\sqrt n\\\\
\therefore 可以从x = \sqrt n 开始,计算x^2-n为完全平方数即可求出x,y \\\\
然后求得a,b
$$

**exp**

```python
from gmpy2 import iroot
def Fermat_factorization(num):
    x,flag_x = iroot(num,2)
    if pow(x,2)<num:
        x+=1
    while True:
        temp = pow(x,2) - num
        y,flag_y = iroot(temp,2)
        if flag_y:
            break
        x+=1
    return int((x+y)),int((x-y))   
```

**结果**

```
10
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x08\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00will get'
```



## frame2,9,16

Pollard's p-1 algorithm

**前置**

费马小定理:  若$p$是素数,且$p \nmid a$, 则$a^{p-1} \equiv 1 \pmod p$

B-Smooth数: 如果一个整数的所有素因子都不大于B,则称该整数为B-Smooth数

**原理**

条件

- 设大整数的一个因子是p, p-1需要是B-Smooth的

$$
设p-1是B-Smooth的,即p-1 = p_1 p_2\cdots p_n \ (\forall 1 \leq i \leq n,p_i \leq B) \\\\
若p_i两两不同,则p_1 p_2\cdots p_n \mid B! \\\\
即 (p-1) \mid B! , B! = k(p-1) \\\\
\therefore a^{B!} \equiv a^{k(p-1)} \equiv 1 \pmod p \\\\
$$

分解过程
$$
假设n = pq\\\\
计算gcd = gcd(a^{B!}-1,n) \\\\
如果 0 < gcd < n, 则gcd为n的一个因子
$$

**算法**

- 设置$ a = 2$(或者其他方便的数字)

- 循环$j = 2,3,\cdots B$

	> 若$p_1,p_2,\cdots,p_n$这些素数是随机在小s于B的数中选的,那么最大的素数大概率要大于0.8B
	>
	> 因此在$j<0.8B$之前不计算gcd,可以节省时间

	- 令$a \equiv a^j \pmod n$
	- 计算$d = gcd(a-1,n)$
	- 如果$1<d<n$, 则返回$d$

**exp**

优化前

```python
from Crypto.Util.number import GCD
def pollard_p_1(n,B):
    a = 2
    d = 0
    for j in range(2,B+1):
        a = pow(a, j, n)
        d = GCD(a-1, n)
        if 1<d<n:
            return d
```

优化后

```python
from Crypto.Util.number import GCD
def pollard_p_1(n,B):
    a = 2
    false_range = int(0.8*B)
    for j in range(2,false_range):
        a = pow(a, j, n)

    d = 0
    for j in range(false_range,B+1):
        a = pow(a, j, n)
        d = GCD(a-1, n)
        if 1<d<n:
            return d
```

**结果**

```
2
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x06\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00 That is'
19
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x05\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00instein.'
6
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x07\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00 "Logic '
```



## frame3,8,12,16,20

Broadcast Attack

**前提条件**

e很小

大于e个人,使用相同的e对同一m进行加密,否则无法正确解密
$$
\because m<n \\\\
\therefore m^e<N = n_1\cdot n_2 \cdots \cdot n_e \\\\
$$

**攻击原理**

选取了相同的加密指数 e（这里取 e=3），对相同的明文 m 进行了加密并进行了消息的传递，那么有：
$$
c_1 \equiv m^3 \pmod {n_1} \\\\
c_2 \equiv m^3 \pmod {n_2} \\\\
c_3 \equiv m^3 \pmod {n_3} \\\\
$$
对上述等式运用中国剩余定理，在 e=3 时，可以得到：
$$
N = n_1\cdot n_2\cdot n_3 \\\\
c_x \equiv m^3 \pmod N
$$
通过对 $c_x$ 进行三次开方就可以求得明文

**结果**

```
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00t is a f'
```



## frame7,11,15

Known High Bits Message Attack

**前提条件**

- 已知明文 $m$ 的高位,记为 $m_0$
- $e$比较小

**原理**
$$
\because c \equiv (m_0+x)^e \pmod n \\\\
set \  f(x) = (m+x)^e - c \\\\
\therefore 存在 x , \  s.t. f(x) = k\cdot n (k = 0,1,2,...)
$$
求解出x后,依据coppersmith定理,可以求出剩下的所有明文部分

**exp**

> 在本地的sage环境下运行
>
> or
>
> [sage在线运行环境](https://sagecell.sagemath.org/)

```python
n = 155266493936043103849855199987896813716831986416707080645036022909153373110367007140301635144950634879983289720164117794783088845393686109145443728632527874768524615377182297125716276153800765906014206797548230661764274997562670900115383324605843933035314110752560290540848152237316752573471110899212429555149
c = 124929943232081828105808318993257526364596580021564021377503915670544445679836588765369503919311404328043203272693851622132258819278328852726005776082575583793735570095307898828254568015886630010269615546857335790791577865565021730890364239443651479580968112031521485174068731577348690810906553798608040451024
e = 3

# 枚举可能的tag
m_lst = []
s = "9876543210abcdef0000000b00000000000000000000000000000000000000000000000000000000000000000000000000000000000000002e20496d6167696e"
for i in "1234567890abcdef":
    m_lst.append(eval('0x'+s[:23]+i+s[24:]))

# 破解
for m in m_lst:
    # 获取m中已知部分,未知部分用0替代
    nbits = 512
    kbits = 64
    mbar = m&(pow(2,nbits)-pow(2,kbits))

    #Zmod(n):Zmod代表这是一个整数域中的n模环
    #PolynomialRing：这个就是说建立多项式环
    #.<X>：指定一个变量
    PR.<x> = PolynomialRing(Zmod(n))
    f = (mbar + x)**e - c
    # small_roots ( self , X = None , beta = 1.0 , epsilon = None , ** kwds )
    # X – 根的绝对边界
    roots = f.small_roots()
    if roots: 
        print(bytes.fromhex(hex(mbar+roots[0])[2:]))
        break
```

**结果**

```
- 7
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x02\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00amous sa'
- 11
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x03\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00ying of '
- 15
b'\x98vT2\x10\xab\xcd\xef\x00\x00\x00\x04\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00Albert E'
```



## frame0-5,7-18,20

**原理**

线性同余方程为 $lcg[i]=(a*lcg[i-1]+b)%m$

**过程**

16bit为一组

```python
# 使用f1的p作为例子
p = bin(7273268163465293471933643674908027120929096536045429682300347130226398442391418956862476173798834057392247872274441320512158525416407044516675402521694747)[2:]
print(len(p))
block_size = 16
chunk = [p[i:i+block_size] for i in range(0,len(p),block_size)]
print(chunk)
print(len(chunk))
```

```
512
['1000101011011110', '1111111010000101', '1110001110100000', '1000101100011111', '0101101100110010', '0000011001001001', '1111011000010100', '1101101010000011', '1000110011000110', '1011011001001101', '1110101111001000', '0010110000100111', '1111001110011010', '0101001010010001', '1011100010111100', '0110010000001011', '1010001110101110', '0101111100010101', '1001000011110000', '1010011000101111', '1111000100000010', '1001111111011001', '1110100001100100', '0101011010010011', '0110111110010110', '0001100011011101', '0111001100011000', '0001100100110111', '1111001101101010', '0000111000100001', '0010010100001100', '1101001000011011']
32
```

计算a,b

```python
# calculate the a,b
x = int(chunk[0],2)
y = int(chunk[1],2)
z = int(chunk[2],2)
m = pow(2,block_size)
a = (z-y)*inverse(y-x,m)%m
b = (y-a*x)%m
print("a = {},b = {}".format(a,b-m))
a = 365,b = -1
```

验证a,b是否正确

```python
# verify the a,b
x = int(chunk[0],2)
print(x,"init",end="\t")
a = 365
b = -1
m = pow(2,block_size)
for i in range(1,len(chunk)):
    y = (a*x+b)%m
    x = y
    if y == int(chunk[i],2):
        print(y,"True",end="\t")
    else:
        print("false",end="\t")
```

```
35550 init	65157 True	58272 True	35615 True	23346 True	1609 True	62996 True	55939 True	36038 True	46669 True	60360 True	11303 True	62362 True	21137 True	47292 True	25611 True	41902 True	24341 True	37104 True	42543 True	61698 True	40921 True	59492 True	22163 True	28566 True	6365 True	29464 True	6455 True	62314 True	3617 True	9484 True	53787 True
```

**exp**

```python
def crack_PRG(n):
    a = 365
    b = -1
    m = pow(2,16)
    # j: RandSeed
    for j in range(m):
        x = j
        p = bin(x)[2:].zfill(16)
        for _ in range(63):
            y = (a*x+b)%m
            x = y
            p += bin(x)[2:].zfill(16)
            if n%int(p,2) == 0:
                return int(p,2)
            """
            p的长度不是16的整数倍,所以需要截断处理
            效率极低,跑全部frame时,可将下述代码注释后执行
            跑特定frame时,取消注释
            """
#             if len(p) == 1024:
#                 for plen in range(513,576):
#                     temp = p[:plen]
#                     if n%int(temp,2) == 0:
#                         return int(temp,2)
```



## 参考

[RSA大礼包](https://www.tr0y.wang/2017/11/06/CTFRSA/#%E4%BD%8E%E8%A7%A3%E5%AF%86%E6%8C%87%E6%95%B0%E6%94%BB%E5%87%BB)

[coppersmith](https://nonuplebroken.com/2019/11/21/RSA%20Coppersmith%E7%9B%B8%E5%85%B3%E6%94%BB%E5%87%BB/#factoring-with-high-bits-known)

[ctf-wiki](https://ctf-wiki.org/crypto/asymmetric/rsa/rsa_coppersmith_attack/#basic-broadcast-attack)

[lcg线性同余随机数生成器](https://weichujian.github.io/2020/05/28/lcg%E7%BA%BF%E6%80%A7%E5%90%8C%E4%BD%99%E9%9A%8F%E6%9C%BA%E6%95%B0%E7%94%9F%E6%88%90%E5%99%A8/)