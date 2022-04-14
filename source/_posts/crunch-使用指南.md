---
title: crunch 使用指南
date: 2019-12-10 19:35:51
categories: vm
tags:
- secure

---

# crunch 使用指南

## 参数详解

min  设定最小字符串长度
max  设定最大字符串长度

options
-b   指定文件输出的大小，避免字典文件过大  
-c   指定文件输出的行数，即包含密码的个数
-d   限制相同元素出现的次数
-e   定义停止字符，即到该字符串就停止生成
-f   调用库文件（/etc/share/crunch/charset.lst）
-i   改变输出格式，即aaa,aab -> aaa,baa
-I   通常与-t联合使用，表明该字符为实义字符
-m   通常与-p搭配
-o   将密码保存到指定文件
-p   指定元素以组合的方式进行
-q   读取密码文件，即读取pass.txt
-r   定义重某一字符串重新开始
-s   指定一个开始的字符，即从自己定义的密码xxxx开始
-t   指定密码输出的格式
-u   禁止打印百分比（必须为最后一个选项）
-z   压缩生成的字典文件，支持gzip,bzip2,lzma,7z  

## 特殊字符

%   代表数字
^   代表特殊符号
@   代表小写字母
,   代表大写字符 

### 使用案例

1. 生成一个字典库 :5位的6个小写字母的随机排列组合,可以生成67 MB这么大的字典文件

```
$ crunch 5 5 -b 20mib -o START
```

2. 生成一个字典文件，用自己指定的字符:默认为26个小写字母为元素的所有组合

 ```
$ crunch 1 3 abc
 ```

3. 通过-l参数来使@,%^等特殊字符输出

```
$ crunch 7 7 -t p@ss,%^ -l a@aaaaa
```

4. -o参数也可使用>>来简化

```
$ crunch 4 4 -d 2@ -t @@@% >> test.txt
```

5. 生成10位密码，并指定格式

```
$ crunch 10 10 -t @@@^%%%%^^ -d 2@ -d 3% -b 20mb -o START
```

格式为三个小写字母+一个符号+四个数字+两个符号，限制每个密码至少2种字母和至少3种数字，文件大小为20MB。“-d 2@”表示字母重复最多2次。

-d 数字符号，限制出现相同元素的个数(至少出现元素个数)，“-d 2@”限制小写字母输出像aab和aac，aaa不会产生，因为这是连续3个字母，格式是数字+符号，数字是连续字母出现的次数，符号是限制字符串的字符，例如@,%^(“@”代表小写字母，“,”代表大写字符，“%”代表数字，“^”代表特殊字符)

-t @,%^，指定模式，@,%^分别代表意义如下：

@ 插入小写字母
, 插入大写字母
% 插入数字
^ 插入特殊符号

## 密码库

```
/usr/share/crunch/charset.lst
```

numeric   表示0123456789
Lalpha   表示26位小写字母
Ualpha   表示26位大写字母

### 使用案例

```
$ crunch 1 1 -f /usr/share/crunch/charset.lst  mixalpha-numeric-all-space -o  START  -c  60 
```


 -c 数字 指定写入输出文件的行数，也即包含密码的个数，例如使用字符规则mixalpha-numeric-all-space，生成最小和最大字符串为1的且每一个文件保存60个字符串的密码字典

 -f /path/to/charset.lst charset-name，从charset.lst指定字符集，也即调用密码库文件，比如kali中的charset.lst 在/usr/share/crunch/charset.lst，则参数为“-f /usr/share/crunch/charset.lst”

## 比较有用的命令

1. 生成pass01-pass99所有数字组合

```
$ crunch 6 6 -t pass%%  >>newpwd.txt 
```

2. 生成六位小写字母密码，其中前四位为pass

```
$ crunch 6 6 -t pass@@  >>newpwd.txt 

```

3. 生成六位密码，其中前四位为pass，后二位为大写

```
$ crunch 6 6 -t pass,,  >>newpwd.txt 

```

4. 生成六位密码，其中前四位为pass，后二位为特殊字符

```
$ crunch 6 6 -t pass^^  >>newpwd.txt 

```

5. 制作8位数字字典

```
$ crunch 8 8 charset.lst numeric -o num8.dic 

```

6. 制作6位数字字典

```
$ crunch 6 6  0123456789 –o num6.dic 

```

7. 制作139开头的手机密码字典

```
$ crunch 11 11  +0123456789 -t 139%%%%%%%% -b 20mib -o START

```

