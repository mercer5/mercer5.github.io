---
title: hexo渲染数学公式
date: 2021-10-31 09:15:49
categories: blog
tags: blog
---

# hexo渲染数学公式

## 渲染引擎下载

更换Hexo的markdown渲染引擎

- hexo-renderer-marked: 默认的渲染引擎
- hexo-renderer-kramed: 默认引擎的基础上修改了一些bug，两者比较接近，也比较轻量级。

```undefined
# 卸载原来的
npm uninstall hexo-renderer-marked --save
# 安装新的
npm install hexo-renderer-kramed --save
```

这样行间公式就可以正确渲染了，但是行内公式的渲染还是有问题，因为hexo-renderer-kramed引擎也有语义冲突的问题。



## 配置引擎

接下来到博客根目录下，找到`node_modules\kramed\lib\rules\inline.js`

- 把第11行的escape变量的值做相应的修改：(在原基础上取消了对\,{,}的转义(escape))

  ```js
  //  escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
  escape: /^\\([`*\[\]()#$+\-.!_>])/
  ```

- 同时把第20行的em变量也要做相应的修改。

  ```js
  //  em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/
  ```



## 在主题中开启mathjax开关

以next主题为例, 在主题(Theme)中开启mathjax开关

进入到**主题目录**，找到_config.yml配置文件，把mathjax默认的false修改为true，具体如下：

```yml
# MathJax Support
mathjax:
  enable: true
  per_page: true
```



## 在文章中使用

在文章的Front-matter里打开mathjax开关

```css
---
title: xxx
date: 2016-12-28 21:01:30
tags:
mathjax: true
--
```

之所以要在文章头里设置开关，是因为考虑只有在用到公式的页面才加载 Mathjax，这样不需要渲染数学公式的页面的访问速度就不会受到影响了。