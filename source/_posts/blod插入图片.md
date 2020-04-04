---
title: blod插入图片
date: 2020-04-04 15:16:53
tags: blog
---

# blog插入图片

## 现状

由于微博图床没了,博主只能苦逼的找方法解决博客上所有图片显示失败问题了.

目前有三种方法

- 用dl的方法(百度你值得拥有)继续使用,但还是算了
  1. 太麻烦了,每个图片都要改
  2. 免费的图床风险还是太大,说没就没
- 买服务器,这条路也不准备走~~问就是没钱~~
- 所以只能把图片保存在本地了: )

## 搞起来

### 绝对路径

可以将图片统一放在`source/images`文件夹中，通过markdown语法访问它们。

```
source/images/image.jpg![](/images/image.jpg)
```



### 相对路径

放在文章自己的目录中。文章的目录可以通过配置`_config.yml`来生成。

```
_config.ymlpost_asset_folder: true
```

将`_config.yml`文件中的配置项`post_asset_folder`设为`true`后，执行命令`$ hexo new post_name`，在`source/_posts`中会生成文章`post_name.md`和同名文件夹`post_name`。将图片资源放在`post_name`中，文章就可以使用相对路径引用图片资源了。

```
_posts/post_name/image.jpg![](image.jpg)
```



