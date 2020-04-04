---
title: burpsuit 试水
date: 2020-2-12 21:32:51
categories: web
tags: 
- web
- burp

---

# burpsuit 试水

## 设备

1. kali
2. firefox esr
3. burpsuit

## 不必要的准备

以上设备都是kali自带的

然后我把火狐设置成中文了(英语渣伤不起啊啊啊啊)

以下是设置成中文的步骤

1. 终端执行:`sudo apt -y install firefox-esr-l10n-zh-cn`

2. 设置首选项

   <img src="00822enrly1gbtx6gzl5hj30be0mhwg8.jpg" alt="Snipaste_2020-02-12_21-01-36.png" style="zoom:67%;" />

3. 语言选项

   <img src="00822enrly1gbtx6zf6kcj30rq077jrs.jpg" alt="Snipaste_2020-02-12_21-02-53.png" style="zoom:67%;" />

4. 选择语言

   <img src="00822enrly1gbtx798ju0j30jb0990t8.jpg" alt="Snipaste_2020-02-12_21-03-28.png" style="zoom:67%;" />

5. 加入并确认

   <img src="00822enrly1gbtx7pzid9j30j509hgm3.jpg" alt="Snipaste_2020-02-12_21-04-05.png" style="zoom:67%;" />



## 设置代理及证书

1. 菜单->首选项->常规->拉到最下面->网络设置

   <img src="00822enrly1gbtxlbobp7j30pv0j2768.jpg" alt="Snipaste_2020-02-12_21-09-50.png" style="zoom:67%;" />

2. 设置代理

   <img src="00822enrly1gbtxlp4s27j30ly0a3gmq.jpg" alt="Snipaste_2020-02-12_21-10-37.png" style="zoom:67%;" />

3. 打开bp

4. 在火狐中输入:http://burp/       **必须先打开bp**

5. 下载

   <img src="00822enrly1gbtxlwyuqwj30n203ojrf.jpg" alt="Snipaste_2020-02-12_21-15-28.png" style="zoom:67%;" />

6. 保存

   <img src="00822enrly1gbtxmfkusgj30dy0ahdgk.jpg" alt="Snipaste_2020-02-12_21-16-10.png" style="zoom:67%;" />

7. 在查看证书中确认

   <img src="00822enrly1gbtxmpddtpj30m50d475n.jpg" alt="Snipaste_2020-02-12_21-17-28.png" style="zoom:67%;" />

8. 导入,选中下载的文件,全点信任,ojbk

