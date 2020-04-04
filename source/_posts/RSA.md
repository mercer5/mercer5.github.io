---
title: RSA算法
date: 2019-12-10 13:06:51
categories: 密码学
tag:
- 密码学
- 算法
- 安全

---

# RSA算法

## 算法

1. 选择两个质数q,p

   eg:61,53

2. `n(模)=q*p`

   eg:3233

3. 计算n的欧拉函数φ(n)

   φ(n) = (p-1)(q-1)

   eg:φ(3233)=60×52=3120

4. 随机选择一个整数e，条件是1< e < φ(n)，且e与φ(n) 互质

   eg:在1到3120之间，随机选择了17。（实际应用中，常常选择65537。）

5. 计算e对于φ(n)的模反元素d

   所谓"模反元素"就是指有一个整数d，可以使得`e*d`被φ(n)除的余数为1

   `e*d mod φ(n) ≡ 1  ` 等价于`e*d - 1 = kφ(n)`

   eg:17x + 3120y = 1  d=2753

6. 将n和e封装成公钥，n和d封装成私钥

   n=3233，e=17，d=2753，所以公钥就是 (3233,17)，私钥就是（3233, 2753）。

   实际应用中，公钥和私钥的数据都采用ASN.1格式表达

## 加密

用公钥 (n,e) 对m进行加密。

这里需要注意，m必须是整数（字符串可以取ascii值或unicode值），且m必须小于n。  

所谓"加密"，就是算出下式的c：

`m^e mod n ≡ c `

eg: 65^17 mod 3233≡ 2790     c=2790

## 解密

可以证明，下面的等式一定成立：

`c^d mod n ≡ m `

也就是说，c的d次方除以n的余数为m。现在，c等于2790，私钥是(3233, 2753)

eg:2790^2753 mod 3233≡ 65 





```python
#知道p,q,e后算d
# coding = utf-8
def computeD(fn, e):
    (x, y, r) = extendedGCD(fn, e)
    #y maybe < 0, so convert it
    if y < 0:
        return fn + y
    return y

def extendedGCD(a, b):
    #a*xi + b*yi = ri
    if b == 0:
        return (1, 0, a)
    #a*x1 + b*y1 = a
    x1 = 1
    y1 = 0
    #a*x2 + b*y2 = b
    x2 = 0
    y2 = 1
    while b != 0:
        q = a / b
        #ri = r(i-2) % r(i-1)
        r = a % b
        a = b
        b = r
        #xi = x(i-2) - q*x(i-1)
        x = x1 - q*x2
        x1 = x2
        x2 = x
        #yi = y(i-2) - q*y(i-1)
        y = y1 - q*y2
        y1 = y2
        y2 = y
    return(x1, y1, a)
p = 
q = 
e = 
n = p * q
fn = (p - 1) * (q - 1)
d = computeD(fn, e)
print(d)

```

