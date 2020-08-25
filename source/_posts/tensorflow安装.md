---
title: tensorflow安装
date: 2020-08-25 14:24:56
categories: python
tags: 
- python
- tensorflow
---

# tensorflow安装



> 想和小伙伴们搞一个机器学习相关的星火杯产品
>
> 所以从今天起要学习一下tensorflow啦
>
> 因为anaconda对数据处理的支持很大,所以就决定是他啦



## 前置措施

为了避免在下载过程中,因网速太慢~~(尤其是那该死的校园网)~~,中断连接导致重下,还是换个源比较好

以下两个都是清华源

1. 换pip源

   ```
   pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
   ```

2. 换conda源

   ```
   # 1. 设置
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
   # 2. 生效
   conda config --set show_channel_urls yes
   ```



## tensorflow安装步骤

> 参考: https://www.kesci.com/home/project/5e0e10332823a10036b24d67

1. 打开 Anaconda Prompt 终端，建立一个 conda 计算环境

   ```
   conda create -n tensorflow python=3.5
   ```

2. 激活 tensorflow 环境

   ```
   activate tensorflow
   ```

   注意:只有出现以下划线部分,才算进入环境,base是默认环境

   tensorflow是我们第一步安装的

   如果没有,见下面 **问题解决->环境无法激活** 部分

   ![image-20200825112438072](image-20200825112438072.png)

3. 安装 tensorflow 模块

   ```
   pip3 install tensorflow
   ```

   如果出现timeout之类的错误,见下 **问题解决->timeout ** 部分

4. 安装 keras 模块

   ```
   pip3 install keras
   ```

5. 将 tensorflow 嵌入到 jupyter

   ```
   conda install ipython
   conda install jupyter
   ipython kernelspec install-self --user
   jupyter kernelspec install-self --user
   ```



## tensorflow使用方法

1. 打开 `Anaconda Prompt` 终端

2. 激活 tensorflow 环境

   ```
   activate tensorflow
   ```

3. 进入 tensorflow 环境之后,输入:

   ```
   jupyter notebook
   ```

4. 关闭 tensorflow 环境,输入:

   ```
   deactive tensorflow
   ```



## 检测tensorflow的安装

```python
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
hello = tf.constant("hello,tensorflow")
sess= tf.compat.v1.Session()
print(sess.run(hello))
```

![image-20200825115632962](image-20200825115632962.png)



## 问题解决

### 环境无法激活

> 参考: https://www.jianshu.com/p/beccb78a7c7a

1. 下载一个开源库

   ```
   conda install -n root -c pscondaenvs pscondaenvs
   ```

2. 更改Windows PowerShell配置

   ```
   Set-ExecutionPolicy RemoteSigned
   ```

   

### timeout

1. 重新换源

   ```
   pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
   ```

   新装了新环境后好像pip源都是新的了

   反正换源后速度正常

2. 升级一下pip

   ```
   python -m pip install --upgrade pip
   ```

   换源后重新下,然后说我pip版本太低了,如有需求就用上述命令更新

3. 然后发现我还是timeout了

   ~~所以我解决了啥啊?先放放,先放放~~

4. 哦,好的,是校园网的问题

   换热点就可以了......