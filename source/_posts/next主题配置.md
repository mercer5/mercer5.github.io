---
title: next主题配置
date: 2020-04-08 13:54:40
categories: blog
tags: blog
---
# next主题配置

## Scheme

外观

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

Scheme 的切换通过更改 **主题配置文件**，搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释符号去除即可。

eg:选择 Pisces Scheme

```
#scheme: Muse
#scheme: Mist
scheme: Pisces
```



## 语言

编辑 **站点配置文件**， 将 `language` 设置成你所需要的语言。建议明确设置你所需要的语言，例如选用简体中文，配置如下：

```
language: zh-Hans
```

目前支持的语言有

| 语言         | 代码                 | 设定示例                            |
| :----------- | :------------------- | :---------------------------------- |
| English      | `en`                 | `language: en`                      |
| 简体中文     | `zh-Hans`            | `language: zh-Hans`                 |
| Français     | `fr-FR`              | `language: fr-FR`                   |
| Português    | `pt`                 | `language: pt` or `language: pt-BR` |
| 繁體中文     | `zh-hk` 或者 `zh-tw` | `language: zh-hk`                   |
| Русский язык | `ru`                 | `language: ru`                      |
| Deutsch      | `de`                 | `language: de`                      |
| 日本語       | `ja`                 | `language: ja`                      |
| Indonesian   | `id`                 | `language: id`                      |
| Korean       | `ko`                 | `language: ko`                      |



## 菜单

菜单配置包括三个部分，第一是菜单项（名称和链接），第二是菜单项的显示文本，第三是菜单项对应的图标。 NexT 使用的是 [Font Awesome](http://fontawesome.io/) 提供的图标

编辑 **主题配置文件**，修改以下内容：

1. 设定菜单内容，对应的字段是 `menu`。 菜单内容的设置格式是：`item name: link || icon_name`。其中 `item name `是一个名称，这个名称并不直接显示在页面上，她将用于匹配图标以及翻译;icon_name是**font awesome** 的图标名

   **这里有一个问题,就是有图标名就没法进入正确的网页;必须把||及后面的删掉才行**

   菜单示例配置

   ```
   menu:
     home: / 
     #about: /about/ 
     tags: /tags/ 
     categories: /categories/ 
     archives: /archives/ 
     #schedule: /schedule/ || calendar
     #sitemap: /sitemap.xml || sitemap
     #commonweal: /404/ || heartbeat
   ```

   除了 `home`， `archives` , `/`后面都需要手动创建这个页面

   NexT 默认的菜单项有（标注 的项表示需要手动创建这个页面）：

   | 键值       | 设定值                    | 显示文本（简体中文） |
   | :--------- | :------------------------ | :------------------- |
   | home       | `home: /`                 | 主页                 |
   | archives   | `archives: /archives`     | 归档页               |
   | categories | `categories: /categories` | 分类页               |
   | tags       | `tags: /tags`             | 标签页               |
   | about      | `about: /about`           | 关于页面             |
   | commonweal | `commonweal: /404.html`   | 公益 404             |

2. 设置菜单项的显示文本。在第一步中设置的菜单的名称并不直接用于界面上的展示。Hexo 在生成的时候将使用 这个名称查找对应的语言翻译，并提取显示文本。这些翻译文本放置在 NexT 主题目录下的 `languages/{language}.yml` （`{language}` 为你所使用的语言）。

   以简体中文为例，若你需要添加一个菜单项，比如 `something`。那么就需要修改简体中文对应的翻译文件 `languages/zh-Hans.yml`，在 `menu` 字段下添加一项：

   ```
   menu:
     home: 首页
     archives: 归档
     categories: 分类
     tags: 标签
     about: 关于
     search: 搜索
     commonweal: 公益404
     something: 有料
   ```

3. 设定菜单项的图标，对应的字段是 `menu_icons`。`enable` 可用于控制是否显示图标，你可以设置成 `false` 来去掉图标。

   菜单图标配置示例

   ```
   menu_icons:
     enable: true
   ```

   在菜单图标开启的情况下，如果菜单项与菜单未匹配（没有设置或者无效的 Font Awesome 图标名字） 的情况下，NexT 将会使用 作为图标。



## 侧栏

### 主体

默认情况下，侧栏仅在文章页面（拥有目录列表）时才显示，并放置于右侧位置。 可以通过修改 **主题配置文件** 中的 `sidebar` 字段来控制侧栏的行为。侧栏的设置包括两个部分，其一是侧栏的位置， 其二是侧栏显示的时机。

1. 设置侧栏的位置，修改 `sidebar.position` 的值，支持的选项有：

   - left - 靠左放置
   - right - 靠右放置

   目前仅 Pisces Scheme 支持 `position` 配置。

2. 设置侧栏显示的时机，修改 `sidebar.display` 的值，支持的选项有：

   - `post` - 默认行为，在文章页面（拥有目录列表）时显示
   - `always` - 在所有页面中都显示
   - `hide` - 在所有页面中都隐藏（可以手动展开）
   - `remove` - 完全移除

   ```
   sidebar:
     # Sidebar Position, available value: left | right (only for Pisces | Gemini).
     position: left
     #position: right
   
     Sidebar Display, available value (only for Muse | Mist):
     
     #display: post    // 默认显示方式
     #display: always  // 一直显示
     display: hide     // 初始隐藏
     #display: remove  // 移除侧边栏
   
   ```


### 社交链接

打开 `themes/next/_config.yml` 文件,搜索关键字 `social` ,然后添加社交站点名称与地址即可。



### 社交图标

打开 `themes/next/_config.yml` 文件,搜索关键字 `social_icons` ，添加社交站点名称（注意大小写）图标，[Font Awesome](http://fontawesome.dashgame.com/)图标地。



## 头像

编辑 **主题配置文件**， 修改字段 `avatar`,删掉前面的#号,值设置成头像的链接地址。其中，头像的链接地址可以是：

| 地址             | 值                                                           |
| :--------------- | :----------------------------------------------------------- |
| 完整的互联网 URI | `http://example.com/avatar.png`                              |
| 站点内的地址     | 将头像放置主题目录下的 `source/uploads/` （新建 uploads 目录若不存在） 配置为：`avatar: /uploads/avatar.png`或者 放置在 `source/images/` 目录下 配置为：`avatar: /images/avatar.png` |



## 添加分类和标签

### 分类

#### 生成“分类”页并添加tpye属性

打开命令行，进入博客所在文件夹。执行命令

```
$ hexo new page categories
```

成功后会提示：

```
INFO  Created: ~/Documents/blog/source/categories/index.md
```

根据上面的路径，找到`index.md`这个文件，打开后默认内容是这样的：

```
---
title: categories
date: 2019-12-10 12:51:51
---
```

添加`type: "categories"`到内容中，添加后是这样的：

```
---
title: categories
date: 2019-12-10 12:51:51
type: "categories"
---
```

保存并关闭文件。

#### 给文章添加“categories”属性

打开需要添加分类的文章，为其添加categories属性。下方的`categories: web前端`表示添加这篇文章到“web前端”这个分类。注意：hexo一篇文章只能属于一个分类，也就是说如果在“- web前端”下方添加“-xxx”，hexo不会产生两个分类，而是把分类嵌套（即该文章属于 “- web前端”下的 “-xxx ”分类）。

```
---
title: aircrack-ng
date: 2019-12-10 19:35:51
categories: 虚拟机
---
```

至此，成功给文章添加分类，点击首页的“分类”可以看到该分类下的所有文章。当然，只有添加了`categories: xxx`的文章才会被收录到首页的“分类”中。

### 标签

#### 生成“标签”页并添加tpye属性

打开命令行，进入博客所在文件夹。执行命令

```
$ hexo new page tags
```

成功后会提示：

```
INFO  Created: ~/Documents/blog/source/tags/index.md
```

根据上面的路径，找到`index.md`这个文件，打开后默认内容是这样的：

```
---
title: tages
date: 2019-12-14 10:51:06
---
```

添加`type: "tags"`到内容中，添加后是这样的：

```
---
title: tages
date: 2019-12-14 10:51:06
type: "tags"
---
```

保存并关闭文件。

#### 给文章添加“tags”属性

打开需要添加标签的文章，为其添加tags属性。

```
---
title: aircrack-ng
date: 2019-12-10 19:35:51
categories: 虚拟机
tags:   //标签
- 安全   //在这里!!
---
```

至此，成功给文章添加分类，点击首页的“标签”可以看到该标签下的所有文章。当然，只有添加了`tags: xxx`的文章才会被收录到首页的“标签”中。



## 不显示全文

编辑 **主题配置文件**， 修改字段 `auto_excerpt`

```
auto_excerpt:
  enable: true
  length: 100
```

`enable: true`:表示不完全显示

`length: 100`:表示只显示100字



## 友链

编辑 **主题配置文件**， 修改字段 `Blog rolls`

自己往下填就是了

```
# Blog rolls
links_title: 友情链接 #标题
links_layout: block #布局，一行一个连接
#links_layout: inline
links: #连接
  baidu: http://example.com/
  google: http://example.com/
```



## 显示当前浏览进度

打开 `themes/next/_config.yml` ,搜索关键字 `scrollpercent` ,把 `false` 改为 `true`。

```bash
  # Scroll percent label in b2t button
  scrollpercent: true
```

如果想把 `top`按钮放在侧边栏,打开 `themes/next/_config.yml` ,搜索关键字 `b2t` ,把 `false` 改为 `true`。

```bash
 # Back to top in sidebar
  b2t: true

  # Scroll percent label in b2t button
  scrollpercent: true
```



## 统计功能

在站点的根目录下：

```ruby
$ npm i --save hexo-wordcount
```

打开 `themes/next/_config.yml` ，搜索关键字 `post_wordcount`：

```bash
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  #字数统计
  wordcount: true
  #预览时间
  min2read: true
  #总字数,显示在页面底部
  totalcount: true
  separated_meta: true
```



## 加载条

打开 `themes/next/_config.yml` ，搜索关键字 `pace`：

pace改为true

pace_theme后面选上面#后面中的一个

```
# Progress bar in the top during page loading.
pace: true
# Themes list:
#pace-theme-big-counter
#pace-theme-bounce
#pace-theme-barber-shop
#pace-theme-center-atom
#pace-theme-center-circle
#pace-theme-center-radar
#pace-theme-center-simple
#pace-theme-corner-indicator
#pace-theme-fill-left
#pace-theme-flash
#pace-theme-loading-bar
#pace-theme-mac-osx
#pace-theme-minimal
# For example
# pace_theme: pace-theme-center-simple
pace_theme: pace-theme-center-simple
```



## 背景样式

打开 `themes/next/_config.yml` ，搜索关键字 `pace`：

四个选一个,改动true/false

```
# Canvas-nest
canvas_nest: true

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false
```



## 添加打赏功能

**主题配置文件**，搜索reward关键词，添加打赏的配置信息。

```text
# Reward
# If true, reward would be displayed in every article by default.
# And you can show or hide one article specially through add page variable `reward: true/false`.
reward:
  enable: true  //默认是false，改为true
  comment: 您的支持是对我最大的鼓励
  wechatpay: /images/wechatpay.jpg  #图片链接或图片相对路径,当然也可以是绝对路径
  alipay: /images/alipay.jpg      #同上
```



## 开启版权声明

打开**主题配置文件**,搜索关键字 post_copyright 

```text
post_copyright:
  enable: true  //默认为false
  license: CC BY-NC-SA 3.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/3.0/
```



## 分页问题

网页往下翻的时候发现一个奇奇怪怪的东西

![Snipaste_2020-04-08_13-41-26](Snipaste_2020-04-08_13-41-26.png)

这能忍???

找到`themes\next\layout\_partials\pagination.swig`

修改代码为

```
{% if page.prev or page.next %}
  <nav class="pagination">
    {{
      paginator({
        prev_text: '<',
        next_text: '>',
        mid_size: 1
      })
    }}
  </nav>
{% endif %}
```

然后就正常了

![Snipaste_2020-04-08_13-45-49](Snipaste_2020-04-08_13-45-49.png)

