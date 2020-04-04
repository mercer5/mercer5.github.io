---
title: blog备份
date: 2020-04-04 15:59:06
tags: blog
---

# 备份blog



----

由于`hexo d`上传部署到github的其实是hexo编译后的文件，是用来生成网页的，不包含源文件。

也就是上传的是在本地目录里自动生成的`.deploy_git`里面。

其他文件 ，包括我们写在source 里面的，和配置文件，主题文件，都没有上传到github

由于最近电脑经常出问题,感觉还是乘早把东西都备份一下的好

----



## 分支

1. 在github中,作为blog的仓库新建一个分支

   我建了一个hexo

   ![Snipaste_2020-04-04_15-36-20](Snipaste_2020-04-04_15-36-20.png)

2. 把新建的分支设为默认分支

   这样的话,clone下来的,还有push上去的都只会在hexo分支中进行

   而最开始的master分支,就会只用来存储静态网页

   `setting`-->`branckes`-->`hexo`-->`update`

   ![Snipaste_2020-04-04_15-40-38](Snipaste_2020-04-04_15-40-38.png)



## 本地

1. 在你喜欢的地方新建一个文件夹

2. 打开git bash ,clone一下文件

   ![Snipaste_2020-04-04_15-44-21](Snipaste_2020-04-04_15-44-21.png)

   ```
   git clone <url>
   ```

3. 把clone到本地的文件删到只剩下**.git**文件夹

   ![Snipaste_2020-04-04_15-46-08](Snipaste_2020-04-04_15-46-08.png)

4. 把之前我们写的博客源文件全部复制过来，除了`.deploy_git`。这里应该说一句，复制过来的源文件应该有一个`.gitignore`，用来忽略一些不需要的文件，如果没有的话，自己新建一个，在里面写上如下，表示这些类型文件不需要git：

   ```
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   ```

5. 看看theme/next 下面有没有**.git**文件夹,如果有,把删掉

   因为git不能嵌套上传

6. 文件夹中大概像图片那样就行了(aaaa,那个`.deploy_git`是之后生成的,忽略掉就好)

   ![Snipaste_2020-04-04_15-48-19](Snipaste_2020-04-04_15-48-19.png)



## 上传

```
git add .
git commit –m "backup"
git push
```

然后去github康康,hexo分支应该就变成你文件夹中应该有的东西了

![Snipaste_2020-04-04_15-57-48](Snipaste_2020-04-04_15-57-48.png)

而master分支没有变化



## 注意事项

确保`hexo deploy`推送的是master分支，hexo目录下的_config.yml文件通常会配置deploy推送的目标地址，这个一般在最初使用hexo时，就会配置为master，不用改动：

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/sherlockyb/sherlockyb.github.io.git
  branch: master
```



## 使用

之后就可以在文件夹中快乐的使用了

### 生成网页

```
hexo clean
hexo g
hexo d
```

### 备份源码

```
git add .
git commit -m "description"
git push
```


