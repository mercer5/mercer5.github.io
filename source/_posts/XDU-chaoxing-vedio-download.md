---
title: "XDU_chaoxing_vedio_download"
date: 2022-04-14 14:35:36
categories: 杂七杂八
tags: 
- tools
- XDU
---

> 一切为了下载垃圾学校放在垃圾超星上的垃圾录播


## 准备工作

1. 咱先到超星录播f12抓个包, 在network那里搜一下`m3u8` 

   ![image-20220413110736942](image-20220413110736942.png)

   - 下面两个 `playback.m3u8` 就是视频中的一大一小两个视频
   - 重点在箭头指向的那个

2. 这是个get传参, 将payload复制出来

   ![image-20220413110900303](image-20220413110900303.png)

   ```json
   {
       "type":"2",
   	"videoPath":
       {
            "pptVideo":"...",
            "mobile":"...",
            "teacherTrack":"...",
            "studentFull":"..."
   	}
   }
   ```

   - pptVideo: ppt视图(我们所看到的小的那个视频)
   - mobile: 这个很神奇,会在teacherTrack和pptVideo之间来回跳转, 害挺智能的(我决定以后都下这个了)
   - teacherTrack: 跟踪老师视图(就是我们所看到的大的视频)
   - studentFull: 对准学生的监控录像XD

3. 将复制出来的东西放到 `vedio.json` 里

   ![image-20220414142405437](image-20220414142405437.png)



## 下载

目录结构如下:

![image-20220414142428275](image-20220414142428275.png)



下面是 `vedio_download.py` 的代码, 根据需要修改 var下的变量

- saveName: 保存的文件名
- vedioType: json信息中的4中类型,默认mobile
- threadNumber: 线程数

```python
import requests,json,time,threading,os

# TODO: var
saveName = "test"
vedioType = "mobile"
threadNumber = 20

# func
def getUrlFromJson(vedioType):
    f = open('vedio.json','r')
    return json.load(f).get("videoPath").get(vedioType)

def parseURL(url):
    tmpUrl = url[:url.rfind('/')+1]
    lst_all = requests.get(url).text.split('\n')
    matchList = [i.strip('\r') for i in lst_all if i.find('.ts')!=-1]
    lst = [tmpUrl+i+'\n' for i in matchList]
    f = open('payload.m3u8','w')
    f.writelines(lst)

def downloadTF(urls):
    for i in range(len(urls)):
        start = time.time()
        ts_url = urls[i]
        ts_name = ts_url.split('/')[-1].split('_')[0]
        try:
            r = requests.get(ts_url,stream=True)
        except Exception as e:
            print('error:{}'.format(e.args))
            continue
        
        ts_path = "temp/{:0>4}.ts".format(ts_name)
        with open(ts_path,"wb+") as f:
            for chunk in r.iter_content(chunk_size=1024):
                if chunk:
                    f.write(chunk)
        end = time.time()
        print('{}_{}_time cost: {}'.format(threading.current_thread().name,ts_path,end-start))

def createThread(threadNumber,urls):
    cnt = len(urls)//threadNumber
    threads = []
    for i in range(threadNumber):
        tmp_urls = urls[i*cnt:i*cnt+cnt]
        t = threading.Thread(target=downloadTF,args=[tmp_urls,],name=str(i))
        t.start()
        threads.append(t)
    for t in threads:
        t.join()

# main
url_m3u8 = getUrlFromJson(vedioType)
parseURL(url_m3u8)
cwd = os.getcwd()
os.makedirs(os.path.join(cwd,'temp'),exist_ok=True)
with open('payload.m3u8','r') as f:
        urls = f.readlines()

createThread(threadNumber,urls)

print('download finished ...')
print('start to merge all the file ...')

cmd = r"copy /b .\temp\*.ts .\{}.ts".format(saveName)
os.system(cmd)

# clear
cmd = 'rmdir /s/q temp'
os.system(cmd)
cmd = 'del /q payload.m3u8'
os.system(cmd)
```


