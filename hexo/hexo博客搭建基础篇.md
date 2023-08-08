---
title: hexo 博客搭建基础篇
date: 2023-04-16 00:26:09
description: hexo 博客搭建基础篇
tags:
- 博客
- hexo
categories:
- 博客
swiper_index: 1
---
## Hexo 搭建流程（基础）

**常用命令**  

* `hexo clean`：删除之前生成的文件，若未生成过静态文件，可忽略，缩写：`hexo cl`
* `hexo generate`：生成静态文章，缩写：`hexo g`
* `hexo deploy`：将生成的前端代码推送到远端，缩写：`hexo d`
* `hexo server`：开启本地服务，缩写：`hexo s`



**代替路径**  

假设 hexo 项目建立在 `~/blog`，此时 `~/blog` 就是该项目的根目录  

| 路径术语     | 实际路径                       | 代替写法              |
|--------------|--------------------------------|-----------------------|
| 站点根目录   | ~/blog                         | SiteRoot              |
| 主题根目录   | ~/blog/themes/butterfly        | ThemeRoot             |
| 站点配置文件 | ~/blog/_config.yml             | _config.yml           |
| 主题配置文件 | ~/blog/_config.butterfly.yml   | _config.butterfly.yml |
| 站点资源目录 | ~/blog/source                  | SiteSource            |
| 主题资源目录 | ~/blog/themes/butterfly/source | ThemeSource           |



---



### 基础


#### 安装 Hexo

```bash
npm install -g hexo-cli
```


#### 初始化

在 `SiteRoot` 下执行以下命令  

```bash
hexo init
```

#### 安装依赖

在 `SiteRoot` 下执行以下命令  

```bash
npm install
```

---


#### 部署到 github

**创建存放博客静态文件的仓库**  

在 github 上创建仓库 `username.github.io`  

这里的 `username` 是 github 的用户名  


**安装 git 部署的插件**  

在 `SiteRoot` 下执行以下命令  

```bash
npm install --save hexo-deployer-git
```

**更改配置文件**  
更改 `_config.yml` 的 `deploy` 属性  

```yml
deploy:
  type: git
  repo: git@github.com:threearapig/threearapig.github.io.git
  branch: master
```

**部署**  

在 `SiteRoot` 下执行以下命令  

```bash
hexo cl; hexo g; hexo d
```



#### Vercel 部署

1. Vercel 新建项目



2. 导入刚刚创建的仓库



3. 直接部署



4. 部署完之后可进入控制面板查看状态



5. 进入控制面板点击查看域



6. 添加域名映射



修改 `_config.yml`，并将映射后的域名添加进去  

```yml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://www.threearapig.cc/
```

---



#### Butterfly 主题

在 `SiteRoot` 下执行以下命令  

```base
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

删除 `ThemeRoot` 目录下的 `.git` 目录  
> 不然不好进行版本控制  


修改 `_config.yml` 的主题属性    

```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: butterfly
```


**安装所需插件**  

在 `SiteRoot` 下执行以下命令  

```base
npm install hexo-renderer-pug hexo-renderer-stylus  --save
```

> pug  
> stylus 渲染器  



**关于主题配置文件**  

将 `ThemeRoot` 的 `_config.yml` 复制到 `SiteRoot` 目录下，并更名为 `_config.butterfly.yml`  

在 `SiteRoot` 下执行以下命令  

```bash
cp ./themes/butterfly/_config.yml ./_config.butterfly.yml
```


---



### 主题页面


#### 标签页

在 `SiteRoot` 下执行以下命令  

```bash
hexo new page tags
```

修改 `SiteSource/tags/index.md`   

```md
---
title: tags
date: 2023-4-10 8:00:00
type: "tags"
---
```

---


#### 分类页

在 `SiteRoot` 下执行以下命令  

```bash
hexo new page categories
```

修改 `SiteSource/categories/index.md`   

```md
---
title: categories
date: 2023-4-10 8:00:00
type: "categories"
---
```

---


#### 关于页

在 `SiteRoot` 下执行以下命令  

```bash
hexo new page about
```

修改 `SiteSource/about/index.md`   

```md
---
title: about
date: 2023-4-10 8:00:00
type: "about"
---
```

---


#### 友情链接

在 `SiteRoot` 下执行以下命令  

```bash
hexo new page link
```

修改 `SiteSource/link/index.md`   

```md
---
title: link
date: 2023-4-10 8:00:00
type: "link"
---
```


创建 `SiteSource/_data/link.yml` 文件（如果没有 _data 文件夹，需自行创建）  
并添加一下内容  

```yml
- class_name: 技术支持
  class_desc: 本网站的搭建由以下开源作者提供技术支持
  link_list: 
    - name: Hexo 
      link: https://hexo.io/zh-cn/
      avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
      descr: 快速、简单且强大的网志框架
      siteshot: https://source.fomal.cc/siteshot/hexo.io.jpg
      
- class_name: 友情链接
  class_desc: 一些好朋友
  link_list:
    - name: Fomalhaut🥝
      link: https://fomal.cc/
      avatar: /assets/head.jpg
      descr: Future is now 🍭🍭🍭
      siteshot: https://source.fomal.cc/siteshot/www.fomal.cn.jpg
```

---


#### 404 页面


主题内置了一个简单的 404 页面  

修改 `_config.butterfly.yml` 的 `error_404` 属性  

```yml
# A simple 404 page
error_404:
  enable: true
  subtitle: "页面沒有找到"
  background: 
```





### 主题基础配置

#### 网站资料  

修改 `_config.yml`  

```yml
# Site
title: Departure Station
subtitle: ''
description: 'A Vimer'
keywords:
author: TheJC
language: zh-CN
timezone: ''
```

---


#### 导航菜单  

修改 `_config.butterfly.yml` 的 `menu` 属性  

```yml
menu:
  首页: / || fas fa-home
  归档: /archives/ || fas fa-archive
  标签: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  列表||fas fa-list||hide:
    音乐: /music/ || fas fa-music
    电影: /movies/ || fas fa-video
  链接: /link/ || fas fa-link
  关于: /about/ || fas fa-heart
```

创建 **音乐** 和 **电影** 的页面  

在 `SiteRoot` 下执行以下命令  

```bash
hexo new page music
```

```bash
hexo new page movies
```


---


#### 代码  

**代码高亮主题**  

修改 `_config.butterfly.yml` 的 `highlight_theme` 属性  

```yml
highlight_theme: mac
```

**代码复制**  

修改 `_config.butterfly.yml` 的 `highlight_copy` 属性  

```yml
highlight_copy: true
```

**代码框展开**  

修改 `_config.butterfly.yml` 的 `highlight_shrink` 属性  

```yml
highlight_shrink: false
```

**代码高度限制**  

修改 `_config.butterfly.yml` 的 `highlight_height_limit` 属性  

```yml
highlight_height_limit: 230
```

**禁止代码自动换行**  

修改 `_config.butterfly.yml` 的 `code_word_wrap` 属性  

```yml
code_word_wrap: false
```

---


#### 社交卡片信息

修改 `_config.butterfly.yml` 的 `social` 属性  

```yml
# Social Settings (社交圖標設置)
# formal:
#   icon: link || the description || color
social:
  fab fa-github: https://github.com/threearapig || Github || '#24292e'
  fas fa-envelope: mailto:threearapig@gmail.com || Email || '#4a7dbe'
```

---



#### 网站图标和头像

修改 `_config.butterfly.yml` 的 `favicon` 和 `avatar` 属性  

```yml
# Favicon（網站圖標）
favicon: https://pichost.threearapig.cc/icons/arch.png

# Avatar (頭像)
avatar:
  img: https://pichost.threearapig.cc/avatar/avatar.png
  effect: false
```

---


#### 文章封面图片  

修改 `_config.butterfly.yml`  

```yml
cover:
  # display the cover or not (是否顯示文章封面)
  index_enable: true
  aside_enable: true
  archives_enable: true
  # the position of cover in home page (封面顯示的位置)
  # left/right/both
  position: both
  # When cover is not set, the default cover is displayed (當沒有設置cover時，默認的封面顯示)
  default_cover:
    # - https://i.loli.net/2020/05/01/gkihqEjXxJ5UZ1C.jpg
    - https://pichost.threearapig.cc/wallpapers/1.jpg
    - https://pichost.threearapig.cc/wallpapers/2.jpg
    - https://pichost.threearapig.cc/wallpapers/3.jpg
```

> 当 `default_cover` 有多个选项时，随即选择  


---


#### 文章页相关配置  

**文章 meta 显示**  

修改 `_config.butterfly.yml` 的 `post_meta` 属性  

```yml
post_meta:
  page: # Home Page
    date_type: both # created or updated or both 主頁文章日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 主頁是否顯示分類
    tags: true # true or false 主頁是否顯示標籤
    label: true # true or false 顯示描述性文字
  post:
    date_type: both # created or updated or both 文章頁日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 文章頁是否顯示分類
    tags: true # true or false 文章頁是否顯示標籤
    label: true # true or false 顯示描述性文字
```


**文章版权**  

修改 `_config.butterfly.yml` 的 `post_copyright` 属性  

```yml
post_copyright:
  enable: true
  decode: false
  author_href:
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```


**文章打赏**  

修改 `_config.butterfly.yml` 的 `post_copyright` 属性  

```yml
# Sponsor/reward
reward:
  enable: true
  QR_code:
    - img: https://pichost.threearapig.cc/pay/wechat.jpg
      link:
      text: 微信
    - img: https://pichost.threearapig.cc/pay/alipay.jpg
      link:
      text: 支付宝
```




**文章目录 TOC**  

修改 `_config.butterfly.yml` 的 `toc` 属性  

```yml
toc:
  post: true
  page: false
  number: false
  expand: false
  style_simple: false # for post
  scroll_percent: false
```


**相关文章**  

修改 `_config.butterfly.yml` 的 `related_post` 属性  

```yml
# Related Articles
related_post:
  enable: true
  limit: 6 # Number of posts displayed
  date_type: created # or created or updated 文章日期顯示創建日或者更新日
```



**文章过期提醒**  

修改 `_config.butterfly.yml` 的 `noticeOutdate` 属性  

```yml
# Displays outdated notice for a post (文章過期提醒)
noticeOutdate:
  enable: true
  style: flat # style: simple/flat
  limit_day: 365 # When will it be shown
  position: top # position: top/bottom
  message_prev: It has been
  message_next: days since the last update, the content of the article may be outdated.
```


**文章分页按钮**  

修改 `_config.butterfly.yml` 的 `post_pagination` 属性  

```yml
# post_pagination (分頁)
# value: 1 || 2 || false
# 1: The 'next post' will link to old post
# 2: The 'next post' will link to new post
# false: disable pagination
post_pagination: 2
```


#### 站点复制

修改 `_config.butterfly.yml` 的 `copy` 属性
> 关闭复制时附带版权信息

```yml
# copy settings
# copyright: Add the copyright information after copied content (複製的內容後面加上版權信息)
copy:
  enable: true
  copyright:
    enable: false
    limit_count: 50
```

> 可配置网站是否可以复制、复制的内容是否添加版权信息


#### Footer 设置

修改`_config.butterfly.yml` 的 `footer` 属性  

```yml
# Footer Settings
# --------------------------------------
footer:
  owner:
    enable: true
    since: 2023
  custom_text:
  copyright: false # Copyright of theme and framework
```



#### 侧边栏配置  

**侧边排版**  

修改`_config.butterfly.yml` 的 `aside` 属性  

```yml
aside:
  enable: true
  hide: false
  button: true
  mobile: true # display on mobile
  position: left # left or right
  display:
    archive: true
    tag: true
    category: true
  card_author:
    enable: true
    description:
    button:
      enable: true
      icon: fab fa-github
      text: Follow Me
      link: https://github.com/threearapig
  card_announcement:
    enable: true
    content: 欢迎来到我的小站！
  card_recent_post:
    enable: true
    limit: 5 # if set 0 will show all
    sort: date # date or updated
    sort_order: # Don't modify the setting unless you know how it works
  card_categories:
    enable: false
    limit: 8 # if set 0 will show all
    expand: none # none/true/false
    sort_order: # Don't modify the setting unless you know how it works
  card_tags:
    enable: false
    limit: 40 # if set 0 will show all
    color: false
    orderby: random # Order of tags, random/name/length
    order: 1 # Sort of order. 1, asc for ascending; -1, desc for descending
    sort_order: # Don't modify the setting unless you know how it works
  card_archives:
    enable: false
    type: monthly # yearly or monthly
    format: MMMM YYYY # eg: YYYY年MM月
    order: -1 # Sort of order. 1, asc for ascending; -1, desc for descending
    limit: 8 # if set 0 will show all
    sort_order: # Don't modify the setting unless you know how it works
  card_webinfo:
    enable: true
    post_count: true
    last_push_date: true
    sort_order: # Don't modify the setting unless you know how it works
```

**访问人数**  

修改`_config.butterfly.yml` 的 `busuanzi` 属性  

```yml
# busuanzi count for PV / UV in site
# 訪問人數
busuanzi:
  site_uv: true
  site_pv: true
  page_pv: true
```

**网站运行时间**  

修改`_config.butterfly.yml` 的 `runtimeshow` 属性  

```yml
# Time difference between publish date and now (網頁運行時間)
# Formal: Month/Day/Year Time or Year/Month/Day Time
runtimeshow:
  enable: true
  publish_date: 4/10/2023 00:00:00
```



#### 右下角按钮  


**阅读模式**  

修改`_config.butterfly.yml` 的 `readmode` 属性  

```yml
readmode: true
```


**夜间模式**

修改`_config.butterfly.yml` 的 `darkmode` 属性  

```yml
# dark mode
darkmode:
  enable: true
  # Toggle Button to switch dark/light mode
  button: true
  # Switch dark/light mode automatically (自動切換 dark mode和 light mode)
  # autoChangeMode: 1  Following System Settings, if the system doesn't support dark mode, it will switch dark mode between 6 pm to 6 am
  # autoChangeMode: 2  Switch dark mode between 6 pm to 6 am
  # autoChangeMode: false
  autoChangeMode: 2
  # Set the light mode time. The value is between 0 and 24. If not set, the default value is 6 and 18
  start: # 8
  end: # 22
```



**滚动状态百分比**  

修改`_config.butterfly.yml` 的 `rightside_scroll_percent` 属性  

```yml
# show scroll percent in scroll-to-top button
rightside_scroll_percent: true
```


---


#### 开启主题色

修改 `_config.butterfly.yml` 的 `theme_color` 属性

```yml
theme_color:
  enable: true
  main: "#49B1F5"
  paginator: "#00c4b6"
  button_hover: "#FF7242"
  text_selection: "#00c4b6"
  link_color: "#99a9bf"
  meta_color: "#858585"
  hr_color: "#A4D8FA"
  code_foreground: "#F47466"
  code_background: "rgba(27, 31, 35, .05)"
  toc_color: "#00c4b6"
  blockquote_padding_color: "#49b1f5"
  blockquote_background_color: "#49b1f5"
  scrollbar_color: "#49b1f5"
  meta_theme_color_light: "ffffff"
  meta_theme_color_dark: "#0d0d0d"
```

---


#### 网站背景  

修改 `_config.butterfly.yml` 的 `background` 属性

```yml
# Website Background (設置網站背景)
# can set it to color or image (可設置圖片 或者 顔色)
# The formal of image: url(http://xxxxxx.com/xxx.jpg)
background: url(https://pichost.threearapig.cc/wallpapers/wallpaper1.jpg)
```


---




#### 网站副标题

修改 `_config.butterfly.yml` 的 `subtitle` 属性

```yml
# the subtitle on homepage (主頁subtitle)
subtitle:
  enable: true
  # Typewriter Effect (打字效果)
  effect: true
  # Customize typed.js (配置typed.js)
  # https://github.com/mattboldt/typed.js/#customization
  typed_option:
  # source 調用第三方服務
  # source: false 關閉調用
  # source: 1  調用一言網的一句話（簡體） https://hitokoto.cn/
  # source: 2  調用一句網（簡體） https://yijuzhan.com/
  # source: 3  調用今日詩詞（簡體） https://www.jinrishici.com/
  # subtitle 會先顯示 source , 再顯示 sub 的內容
  source: false
  # 如果關閉打字效果，subtitle 只會顯示 sub 的第一行文字
  sub:
    - "Starting now, everything is in time"
    - "Suffering is the greatest gift God has given"
```

> sub 子属性的选项过多，会轮流播放  



#### 页面加载动画

修改 `_config.butterfly.yml` 的 `preloader` 属性
> 不是很喜欢加载动画  

```yml
# Loading Animation (加載動畫)
preloader:
  enable: false
  # source
  # 1. fullpage-loading
  # 2. pace (progress bar)
  source: 1
  # pace theme (see https://codebyzach.github.io/pace/)
  pace_css_url:
```




#### 开启通知弹窗

修改 `_config.butterfly.yml` 的 `snackbar` 属性

```yml
snackbar:
  enable: true
  position: top-right
  bg_light: '#49b1f5' # The background color of Toast Notification in light mode
  bg_dark: '#1f1f1f' # The background color of Toast Notification in dark mode
```

#### 打开预加载功能

修改 `_config.butterfly.yml` 的 `instantpage` 属性

```yml
# prefetch (預加載)
instantpage: true
```


#### 图片懒加载并启动模糊效果

修改 `_config.butterfly.yml` 的 `lazyload` 属性

```yml
# Lazyload (圖片懶加載)
# https://github.com/verlok/vanilla-lazyload
lazyload:
  enable: true
  field: site # site/post
  placeholder:
  blur: true
```

