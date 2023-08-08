---
title: hexo博客搭建进阶篇
date: 2023-04-16 16:05:01
description: hexo 博客搭建进阶篇
tags:
- 博客
- hexo
categories:
- 博客
swiper_index: 1
---


## Hexo 搭建流程（魔改）

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
### 主题魔改



#### 一图流

新建文件 `SiteSource/css/custom.css`，加入以下内容  

```css
/* 页脚与头图透明 */
#footer {
  background: transparent !important;
}
#page-header {
  background: transparent !important;
}

/* 白天模式遮罩透明 */
#footer::before {
  background: transparent !important;
}
#page-header::before {
  background: transparent !important;
}

/* 夜间模式遮罩透明 */
[data-theme="dark"] #footer::before {
  background: transparent !important;
}
[data-theme="dark"] #page-header::before {
  background: transparent !important;
}
```

在 `_config.butterfly.yml`中的 `inject` 配置项的 `head` 子项加入以下代码，代表引入刚刚创建的 `custom.css` 文件

```yml
inject:
  head:
    - <link rel="stylesheet" href="/css/custom.css" media="defer" onload="this.media='all'">
```

> 在 `_config.butterfly.yml` 中的 `index_img` 和 `footer_bg` 配置项取消头图与页脚图的加载项避免冗余加载  
> ```yml
> # The banner image of home page
> index_img: 
>
> # Footer Background
> footer_bg: false
> ```

---



#### 页脚 Github 徽标

安装相关插件

```bash
npm install hexo-butterfly-footer-beautify --save
```

在 `_config.yml` 中，添加以下内容  

```yml
# footer_beautify
footer_beautify:
  enable:
    timer: true # 计时器开关
    bdage: true # 徽标开关
  priority: 5 #过滤器优先权
  enable_page: all # 应用页面
  exclude: #屏蔽页面
    # - /posts/
    # - /about/
  layout: # 挂载容器类型
    type: id
    name: footer-wrap
    index: 0
  # 计时器部分配置项
  runtime_js: https://blog.threearapig.cc/footer/runtime.js
  runtime_css: https://blog.threearapig.cc/footer/runtime.css
  # 徽标部分配置项
  swiperpara: 0 #若非0，则开启轮播功能，每行徽标个数
  bdageitem:
    - link: https://hexo.io/ #徽标指向网站链接
      shields: https://img.shields.io/badge/Frame-Hexo-blue?style=flat&logo=hexo #徽标API
      message: 博客框架为Hexo_v6.2.0 #徽标提示语
    - link: https://butterfly.js.org/
      shields: https://img.shields.io/badge/Theme-Butterfly-6513df?style=flat&logo=bitdefender
      message: 主题版本Butterfly_v4.3.1
    - link: https://vercel.com/
      shields: https://img.shields.io/badge/Hosted-Vercel-brightgreen?style=flat&logo=Vercel
      message: 本站采用多线部署，主线路托管于Vercel
    - link: https://github.com/
      shields: https://img.shields.io/badge/Source-Github-d021d6?style=flat&logo=GitHub
      message: 本站项目由Github托管
    - link: http://creativecommons.org/licenses/by-nc-sa/4.0/
      shields: https://img.shields.io/badge/Copyright-BY--NC--SA%204.0-d42328?style=flat&logo=Claris
      message: 本站采用知识共享署名-非商业性使用-相同方式共享4.0国际许可协议进行许可
  swiper_css: https://blog.threearapig.cc/swiper/swiper.min.css
  swiper_js: https://blog.threearapig.cc/swiper/swiper.min.js
  swiperbdage_init_js: https://blog.threearapig.cc/swiper/swiperbdage_init.min.js
```



#### 首页分类磁贴

安装相关插件  

```bash
npm i hexo-magnet --save
```

在 `_config.yml` 中，添加以下内容  

```yml
magnet:
  enable: true
  priority: 1
  enable_page: /
  type: categories
  devide: 2
  display:
    - name: 教程
      display_name: 魔改教程
      icon: 📚
    - name: 学习
      display_name: 读书笔记
      icon: 📒
    - name: 随想
      display_name: 胡思乱想
      icon: 💡
  color_setting:
    text_color: black
    text_hover_color: white
    background_color: "#f2f2f2"
    background_hover_color: "#b30070"
  layout:
    type: id
    name: recent-posts
    index: 0
  temple_html: '<div class="recent-post-item" style="width:100%;height: auto"><div id="catalog_magnet">${temple_html_item}</div></div>'
  plus_style: ""
```

> 这里 `name` 必须与 文章分类名字相同  


**黑色模式适配**  

在文件 `SiteSource/css/custom.css` 中添加以下内容  

```css
/* 小冰分类分类磁铁黑夜模式适配 */
/* 一般状态 */
[data-theme="dark"] .magnet_link_context {
  background: #1e1e1e;
  color: antiquewhite;
}
/* 鼠标悬浮状态 */
[data-theme="dark"] .magnet_link_context:hover {
  background: #3ecdf1;
  color: #f2f2f2;
}
```



#### 文章置顶滚动栏

安装相关插件  

```bash
npm install hexo-butterfly-swiper --save
```

在 `~/blog/_config.yml` 中，添加以下内容  

```yml
# hexo-butterfly-swiper
# see https://akilar.top/posts/8e1264d1/
swiper:
  enable: true # 开关
  priority: 5 #过滤器优先权
  enable_page: all # 应用页面
  timemode: date #date/updated
  layout: # 挂载容器类型
    type: id
    name: recent-posts
    index: 0
  default_descr: 再怎么看我也不知道怎么描述它的啦！
    #  swiper_css: https://blog.threearapig.cc/swiper/swiper.min.css #swiper css依赖
    #  swiper_js: https://blog.threearapig.cc/swiper/swiper.min.js #swiper js依赖
    #  custom_css: https://blog.threearapig.cc/swiper/swiperstyle.css # 适配主题样式补丁
    #  custom_js: https://blog.threearapig.cc/swiper/swiper_init.js # swiper初始化方法
  swiper_css: https://npm.elemecdn.com/hexo-butterfly-swiper/lib/swiper.min.css #swiper css依赖
  swiper_js: https://npm.elemecdn.com/hexo-butterfly-swiper/lib/swiper.min.js #swiper js依赖
  custom_css: https://npm.elemecdn.com/hexo-butterfly-swiper/lib/swiperstyle.css # 适配主题样式补丁
  custom_js: https://npm.elemecdn.com/hexo-butterfly-swiper/lib/swiper_init.js # swiper初始化方法
```


#### wowjs 动画

安装对应插件  

```bash
npm install hexo-butterfly-wowjs --save
```

在 `_config.yml` 中，添加以下内容  

```yml
wowjs:
  enable: true #控制动画开关。true是打开，false是关闭
  priority: 10 #过滤器优先级
  mobile: false #移动端是否启用，默认移动端禁用
  animateitem:
    - class: recent-post-item #必填项，需要添加动画的元素的class
      style: animate__zoomIn #必填项，需要添加的动画
      duration: 2s #选填项，动画持续时间，单位可以是ms也可以是s。例如3s，700ms。
      delay: 1s #选填项，动画开始的延迟时间，单位可以是ms也可以是s。例如3s，700ms。
      offset: 100 #选填项，开始动画的距离（相对浏览器底部）
      iteration: 1 #选填项，动画重复的次数
    - class: card-widget
      style: animate__zoomIn
  animate_css: https://blog.threearapig.cc/wowjs/animate.min.css
  wow_js: https://blog.threearapig.cc/wowjs/wow.min.js
  wow_init_js: https://blog.threearapig.cc/wowjs/wow_init.js
```



#### GitCalendar

安装相关插件  

```bash
npm install hexo-filter-gitcalendar --save
```

在 `~/blog/_config.yml` 中，添加以下内容  

```yml
# hexo-filter-gitcalendar
gitcalendar:
  enable: true # 开关
  priority: 5 #过滤器优先权
  enable_page: /about/ # 应用页面
  # butterfly挂载容器
  layout: # 挂载容器类型
    type: id
    name: gitZone
    index: 0
  user: threearapig #git用户名
  apiurl: 'https://gitcalendar.threearapig.cc'	# 这是我的API，最好自己弄一个
  minheight:
    pc: 280px #桌面端最小高度
    mibile: 0px #移动端最小高度
  color: "['#d9e0df', '#c6e0dc', '#a8dcd4', '#9adcd2', '#89ded1', '#77e0d0', '#5fdecb', '#47dcc6', '#39dcc3', '#1fdabe', '#00dab9']" # 目前我在用的
  # "['#e4dfd7', '#f9f4dc', '#f7e8aa', '#f7e8aa', '#f8df72', '#fcd217', '#fcc515', '#f28e16', '#fb8b05', '#d85916', '#f43e06']" #橘黄色调
  # color: "['#ebedf0', '#fdcdec', '#fc9bd9', '#fa6ac5', '#f838b2', '#f5089f', '#c4067e', '#92055e', '#540336', '#48022f', '#30021f']" #浅紫色调
  # color: "['#ebedf0', '#f0fff4', '#dcffe4', '#bef5cb', '#85e89d', '#34d058', '#28a745', '#22863a', '#176f2c', '#165c26', '#144620']" #翠绿色调
  # color: "['#ebedf0', '#f1f8ff', '#dbedff', '#c8e1ff', '#79b8ff', '#2188ff', '#0366d6', '#005cc5', '#044289', '#032f62', '#05264c']" #天青色调
  container: .recent-post-item(style='width:100%;height:auto;padding:10px;') #父元素容器，需要使用pug语法
  gitcalendar_css: https://blog.threearapig.cc/gitcalendar/gitcalendar.css
  gitcalendar_js: https://blog.threearapig.cc/gitcalendar/gitcalendar.js
```


在 `SiteSource/about/index.md` 文件中加入以下内容  

```md
<!-- GitCalendar容器 -->
<div id="gitZone"></div>
```



#### 直达底部按钮

在 `ThemeRoot/layout/includes/rightside.pug` 做以下修改  

```diff
  button#go-up(type="button" title=_p("rightside.back_to_top"))
    i.fas.fa-arrow-up

+ button#go-down(type="button" title="直达底部" onclick="btf.scrollToDest(document.body.scrollHeight, 500)")
+   i.fas.fa-arrow-down
```

#### 顶部渐变色加载条

在 `_config.butterfly.yml` 的 `inject` 配置项加入刚刚的css样式和必须的js依赖  

```yml
inject:
  head:
    - <link rel="stylesheet" href="//blog.threearapig.cc/pace/progress_bar.css" media="defer" onload="this.media='all'">
  bottom: 
    - <script async src="//blog.threearapig.cc/pace/pace.min.js"></script>
```



#### 文章三栏

替换 `ThemeRoot/layout/includes/mixins/post-ui.pug` 文件的内容为  

```pug
mixin postUI(posts)
  each article , index in page.posts.data
    .recent-post-item
      -
        let link = article.link || article.path
        let title = article.title || _p('no_title')
        const position = theme.cover.position
        let leftOrRight = position === 'both'
          ? index%2 == 0 ? 'left' : 'right'
          : position === 'left' ? 'left' : 'right'
        let post_cover = article.cover
        let no_cover = article.cover === false || !theme.cover.index_enable ? 'no-cover' : ''
      -
      .recent-post-content(class=leftOrRight)
        .recent-post-cover-shadow
        .recent-post-cover
          img.article-cover(src=url_for(post_cover) onerror=`this.onerror=null;this.src='`+ url_for(theme.error_img.post_page) + `'` alt=title)
        .recent-post-info
          a.article-title(href=url_for(link) title=title)
            .article-title-link= title
          .recent-post-meta                
            .article-meta-wrap
              if (is_home() && (article.top || article.sticky > 0))
                span.article-meta
                  i.fas.fa-thumbtack.sticky
                  span.sticky= _p('sticky')
                  span.article-meta-separator |
              if (theme.post_meta.page.date_type)
                span.post-meta-date
                  if (theme.post_meta.page.date_type === 'both')
                    i.far.fa-calendar-alt
                    span.article-meta-label=_p('post.created')
                    time.post-meta-date-created(datetime=date_xml(article.date) title=_p('post.created') + ' ' + full_date(article.date))=date(article.date, config.date_format)
                    span.article-meta-separator |
                    i.fas.fa-history
                    span.article-meta-label=_p('post.updated')
                    time.post-meta-date-updated(datetime=date_xml(article.updated) title=_p('post.updated') + ' ' + full_date(article.updated))=date(article.updated, config.date_format)
                  else
                    - let data_type_updated = theme.post_meta.page.date_type === 'updated'
                    - let date_type = data_type_updated ? 'updated' : 'date'
                    - let date_icon = data_type_updated ? 'fas fa-history' :'far fa-calendar-alt'
                    - let date_title = data_type_updated ? _p('post.updated') : _p('post.created')
                    i(class=date_icon)
                    span.article-meta-label=date_title
                    time(datetime=date_xml(article[date_type]) title=date_title + ' ' + full_date(article[date_type]))=date(article[date_type], config.date_format)
              if (theme.post_meta.page.categories && article.categories.data.length > 0)
                span.article-meta
                  span.article-meta-separator |
                  i.fas.fa-inbox
                  each item, index in article.categories.data
                    a(href=url_for(item.path)).article-meta__categories #[=item.name]
                    if (index < article.categories.data.length - 1)
                      i.fas.fa-angle-right.article-meta-link
              if (theme.post_meta.page.tags && article.tags.data.length > 0)
                span.article-meta.tags
                  span.article-meta-separator |
                  i.fas.fa-tag
                  each item, index in article.tags.data
                    a(href=url_for(item.path)).article-meta__tags #[=item.name]
                    if (index < article.tags.data.length - 1)
                      span.article-meta-link #[='•']
              
              mixin countBlockInIndex
                - needLoadCountJs = true
                span.article-meta
                  span.article-meta-separator |
                  i.fas.fa-comments
                  if block
                    block
                  span.article-meta-label= ' ' + _p('card_post_count')
              
              if theme.comments.card_post_count
                case theme.comments.use[0]
                  when 'Disqus'
                    +countBlockInIndex
                      a(href=full_url_for(link) + '#disqus_thread')
                        i.fa-solid.fa-spinner.fa-spin
                  when 'Disqusjs'
                    +countBlockInIndex
                      a(href=full_url_for(link) + '#disqusjs')
                        span.disqus-comment-count(data-disqus-url=full_url_for(link))
                          i.fa-solid.fa-spinner.fa-spin
                  when 'Valine'
                    +countBlockInIndex
                      a(href=url_for(link) + '#post-comment')
                        span.valine-comment-count(data-xid=url_for(link))
                          i.fa-solid.fa-spinner.fa-spin
                  when 'Waline'
                    +countBlockInIndex
                      a(href=url_for(link) + '#post-comment')
                        span.waline-comment-count(id=url_for(link))
                          i.fa-solid.fa-spinner.fa-spin
                  when 'Twikoo'
                    +countBlockInIndex
                      a.twikoo-count(href=url_for(link) + '#post-comment')
                        i.fa-solid.fa-spinner.fa-spin
                  when 'Facebook Comments'
                    +countBlockInIndex
                      a(href=url_for(link) + '#post-comment')
                        span.fb-comments-count(data-href=urlNoIndex(article.permalink))
                  when 'Remark42'
                    +countBlockInIndex
                      a(href=url_for(link) + '#post-comment')
                        span.remark42__counter(data-url=urlNoIndex(article.permalink))
                          i.fa-solid.fa-spinner.fa-spin
                  when 'Artalk'
                    +countBlockInIndex
                      a(href=url_for(link) + '#post-comment')
                        span.artalk-count(data-page-key=url_for(link))
                          i.fa-solid.fa-spinner.fa-spin      
        a.article-content(href=url_for(link) title=title)
          //- Display the article introduction on homepage
          case theme.index_post_content.method
            when false
              - break
            when 1
              .article-content-text!= article.description
            when 2
              if article.description
                .article-content-text!= article.description
              else
                - const content = strip_html(article.content)
                - let expert = content.substring(0, theme.index_post_content.length) 
                - content.length > theme.index_post_content.length ? expert += ' ...' : ''
                .article-content-text!= expert
            default
              - const content = strip_html(article.content)
              - let expert = content.substring(0, theme.index_post_content.length) 
              - content.length > theme.index_post_content.length ? expert += ' ...' : ''
              .article-content-text!= expert      
        .recent-post-arrow

    if theme.ad && theme.ad.index
      if (index + 1) % 3 == 0
        .recent-post-item.ads-wrap!=theme.ad.index
```

新建目录 `ThemeSource/css/_index_card_style`，并在此目录下创建文件  

`slidecard.styl` 的内容为  

```styl
//default color:
:root
  --recent-post-bgcolor: rgba(255, 255, 255, 0.7)  //默认背景
  --article-content-bgcolor: #49b1f5 //描述版块背景
  --recent-post-arrow: #ffffff //箭头配色
  --recent-post-cover-shadow: #ffffff //封面遮罩层配色，建议和默认值的颜色相对应。
  --recent-post-transition: all 0.5s cubic-bezier(0.59, 0.01, 0.48, 1.17)  //动画效果。不了解的不要改动
[data-theme="dark"]
  --recent-post-bgcolor: rgba(35,35,35,0.5)
  --article-content-bgcolor: #99999a
  --recent-post-arrow: #37e2dd
  --recent-post-cover-shadow: #232323
// 默认的首页卡片容器布局
.recent-posts
  padding 0 15px 0 15px
  height fit-content
  .recent-post-item
    margin-bottom 15px
    width 100%
    background var(--recent-post-bgcolor)
    overflow hidden
    border-radius 15px
    .recent-post-content
      display flex
      background var(--recent-post-bgcolor)
      position relative
      .recent-post-cover
        display flex
        background transparent
      .recent-post-info
        display flex
        background transparent
        flex-direction column
        justify-content center
        align-items center
        .article-title
          height 50%
          display: flex
          text-align: center
          align-items: center
          justify-content: flex-end
          flex-direction: column
          .article-title-link
            color: var(--text-highlight-color)
            transition: all .2s ease-in-out
            display: -webkit-box;
            -webkit-box-orient: vertical;
            overflow: hidden;
            &:hover
              color: $text-hover
        .recent-post-meta
          height 50%
          display: flex
          text-align: center
          align-items: center
          justify-content: flex-start
          flex-direction: column
          .article-meta-wrap
            color #969797
            display: -webkit-box;
            -webkit-box-orient: vertical;
            overflow: hidden;
            a
              color: var(--text-highlight-color)
              transition: all .2s ease-in-out
              color #969797
              &:hover
                color: $text-hover
      .article-content
        display flex
        text-align: center
        flex-direction row
        align-items center
        justify-content center
        .article-content-text
          display -webkit-box
          -webkit-box-orient vertical
          text-overflow: ellipsis
          overflow hidden
          color #fff
          text-shadow 1px 2px 3px #000
          &::before
            content "❝"
            font-size 20px
          &::after
            content "❞"
            font-size 20px
    &.ads-wrap
      display: block !important
      height: auto !important
// PC端滑动卡片样式
@media screen and (min-width:1069px)
  .recent-posts
    padding 0 15px 0 15px
    .recent-post-item
      .recent-post-content
        position relative
        height 200px
        width 100%
        transition var(--recent-post-transition)
        &:hover
          .recent-post-cover-shadow
            width 10.1%
            transition var(--recent-post-transition)
          .recent-post-cover
            width 10%
            transition var(--recent-post-transition)
          .article-content
            width calc(30% + 80px)
            transition var(--recent-post-transition)
            .article-content-text
              opacity 1
          .recent-post-arrow
            transition var(--recent-post-transition)
        .recent-post-cover-shadow
          z-index: 1
          transition var(--recent-post-transition)
          position: absolute
          height 200px
          width 40%
        .recent-post-cover
          height 200px
          width 40%
          transition var(--recent-post-transition)
          img
            height 100%
            width 100%
            object-fit cover

        .recent-post-info
          height 200px
          width calc(60% - 80px)
          .article-title
            margin: 0px 40px
            font-size 24px
            .article-title-link
              -webkit-line-clamp: 2;
          .recent-post-meta
            margin: 0px 20px
            .article-meta-wrap
              font-size 12px
              -webkit-line-clamp: 3;
        .article-content
          height 200px
          width 90px
          background var(--article-content-bgcolor)
          transition var(--recent-post-transition)
          .article-content-text
            -webkit-line-clamp 4
            transition: var(--recent-post-transition)
            opacity 0
        .recent-post-arrow
          transition var(--recent-post-transition)
          display block
          position absolute
          height 20px
          width 8px
          background var(--recent-post-arrow)
        &.both,
        &.right
          .recent-post-cover-shadow
            left 0
            background linear-gradient(to left, var(--recent-post-cover-shadow), transparent)
          .recent-post-cover
            order: 1
          .recent-post-info
            order: 2
          .article-content
            order: 3
            clip-path polygon(0 50%, 80px 0, 100% 0, 100% 100%, 80px 100%)
            .article-content-text
              margin 20px 40px 20px 80px
          .recent-post-arrow
            order: 4
            left calc(100% - 80px)
            top calc(50% - 10px)
            clip-path polygon(0 10px, 8px 0, 8px 20px)
          &:hover
            .recent-post-arrow
              left calc(100% - 40px)
        &.left
          .recent-post-cover-shadow
            right 0
            background linear-gradient(to right, var(--recent-post-cover-shadow), transparent)
          .recent-post-cover
            order: 4
          .recent-post-info
            order: 3
          .article-content
            order: 2
            clip-path polygon(100% 50%,calc(100% - 80px) 100%,0 100%,0 0,calc(100% - 80px) 0)
            .article-content-text
              margin 20px 80px 20px 40px
          .recent-post-arrow
            order: 1
            left 72px
            top calc(50% - 10px)
            clip-path polygon(0 0, 8px 10px, 0 20px)
          &:hover
            .recent-post-arrow
              left 32px
// 双栏布局卡片自适应适配
@media screen and (min-width:572px) and (max-width:1068px)
  .recent-posts
    padding 0 15px 0 15px
    display flex
    flex-direction row
    flex-wrap wrap
    .recent-post-item
      border-radius 15px
      overflow hidden
      width 47%
      margin 0px 3% 20px 0px
    nav#pagination
      width: 100%
// 手机端单栏布局自适应适配
@media screen and (max-width:572px)
  .recent-posts
    padding 0 15px 0 15px
    .recent-post-item
      border-radius 15px
      overflow hidden
        
// 手机端及双栏卡片样式
@media screen and (max-width:1068px)
  .recent-posts
    .recent-post-item
      .recent-post-content
        flex-direction column
        flex-wrap nowrap
        align-items center
        max-height 350px
        height: auto 
        width 100%
        .recent-post-cover
          width 100%
          height 200px
          clip-path polygon(0 130px,0 0,100% 0,100% 130px,50% 100%)
          img
            height 200px
            width 100%
            object-fit cover
        .recent-post-info
          height 150px
          width 100%
          padding 0px 25px 5px 25px
          .article-title
            margin: 0px 40px
            font-size 18px
            .article-title-link
              -webkit-line-clamp: 2;
          .recent-post-meta
            margin: 0px 20px
            .article-meta-wrap
              font-size 12px
              -webkit-line-clamp: 3;
        .article-content
          position absolute
          height 200px
          width 100%
          background rgba(25,25,25,0.5)
          clip-path polygon(0 130px,0 0,100% 0,100% 130px,50% 100%)
          .article-content-text
            -webkit-line-clamp 3
            font-size 16px
            margin 0px 25px 30px 25px
        .recent-post-arrow
          display block
          background var(--article-content-bgcolor)
          position absolute
          height 10px
          width 20px
          clip-path polygon(0 0,100% 0,50% 100%)
          top 20px
```


`multicard.styl` 的内容为  

```styl
//default color:
:root
  --recent-post-bgcolor: rgba(255, 255, 255, 0.7)  //默认背景
  --article-content-bgcolor: #49b1f5 //描述版块背景
  --recent-post-arrow: #ffffff //箭头配色
  --recent-post-cover-shadow: #ffffff //封面遮罩层配色，建议和默认值的颜色相对应。
  --recent-post-transition: all 0.5s cubic-bezier(0.59, 0.01, 0.48, 1.17)  //动画效果。不了解的不要改动
[data-theme="dark"]
  --recent-post-bgcolor: rgba(35,35,35,0.7)
  --article-content-bgcolor: #99999a
  --recent-post-arrow: #37e2dd
  --recent-post-cover-shadow: #232323
// 默认的首页卡片容器布局
.recent-posts
  padding 0 15px 0 15px
  height fit-content
  .recent-post-item
    margin-bottom 15px
    background var(--recent-post-bgcolor)
    overflow hidden
    border-radius 15px
    .recent-post-content
      display flex
      background var(--recent-post-bgcolor)
      position relative
      .recent-post-cover
        display flex
        background transparent
      .recent-post-info
        display flex
        background transparent
        flex-direction column
        justify-content center
        align-items center
        .article-title
          height 50%
          display: flex
          text-align: center
          align-items: center
          justify-content: flex-end
          flex-direction: column
          .article-title-link
            color: var(--text-highlight-color)
            transition: all .2s ease-in-out
            display: -webkit-box;
            -webkit-box-orient: vertical;
            overflow: hidden;
            &:hover
              color: $text-hover
        .recent-post-meta
          height 50%
          display: flex
          text-align: center
          align-items: center
          justify-content: flex-start
          flex-direction: column
          .article-meta-wrap
            color #969797
            display: -webkit-box;
            -webkit-box-orient: vertical;
            overflow: hidden;
            a
              color: var(--text-highlight-color)
              transition: all .2s ease-in-out
              color #969797
              &:hover
                color: $text-hover
      .article-content
        display flex
        text-align: center
        flex-direction row
        align-items center
        justify-content center
        .article-content-text
          display -webkit-box
          -webkit-box-orient vertical
          text-overflow: ellipsis
          overflow hidden
          color #fff
          text-shadow 1px 2px 3px #000
    &.ads-wrap
      display: block !important
      height: auto !important
  nav#pagination
    width: 100%
// 卡片单元布局样式
.recent-posts
  padding 0 15px 0 15px
  display flex
  flex-direction row
  flex-wrap wrap
  .recent-post-item
    border-radius 15px
    overflow hidden
    .recent-post-content
      flex-direction column
      flex-wrap nowrap
      align-items center
      max-height 350px
      height: auto
      width 100%
      .recent-post-cover
        width 100%
        height 200px
        clip-path polygon(0 130px,0 0,100% 0,100% 130px,50% 100%)
        img
          height 200px
          width 100%
          object-fit cover
      .recent-post-info
        height 150px
        width 100%
        padding 0px 25px 5px 25px
        .article-title
          margin: 0px 40px
          font-size 18px
          .article-title-link
            -webkit-line-clamp: 2;
        .recent-post-meta
          margin: 0px 20px
          .article-meta-wrap
            font-size 12px
            -webkit-line-clamp: 3;
      .article-content
        position absolute
        height 200px
        width 100%
        background rgba(25,25,25,0.5)
        clip-path polygon(0 130px,0 0,100% 0,100% 130px,50% 100%)
        .article-content-text
          -webkit-line-clamp 3
          font-size 16px
          margin 0px 25px 30px 25px
          &::before
            content "❝"
            font-size 20px
          &::after
            content "❞"
            font-size 20px
      .recent-post-arrow
        display block
        background var(--article-content-bgcolor)
        position absolute
        height 10px
        width 20px
        clip-path polygon(0 0,100% 0,50% 100%)
        top 20px
// 三栏布局滑动卡片样式
@media screen and (min-width:1069px)
  .recent-posts
    .recent-post-item
      width 32.3%
      margin 0px 1% 20px 0px
      .recent-post-content
        .recent-post-info
          .article-title
            margin: 0px 5px
            .article-title-link
              -webkit-line-clamp: 1;
          .recent-post-meta
            margin: 0px 5px
            .article-meta-wrap
              -webkit-line-clamp: 2;
// 双栏布局卡片自适应适配
@media screen and (min-width:572px) and (max-width:1068px)
  .recent-posts
    .recent-post-item
      width 47%
      margin 0px 3% 20px 0px
// 单栏布局卡片自适应适配
@media screen and (max-width:572px)
  .recent-posts
    .recent-post-item
      width 100%
```


替换文件 `ThemeSource/css/_page/homepage.styl` 的内容  

```styl
if hexo-config('index_card_style') == 'slidecard'
  @import './_index_card_style/slidecard'
else if hexo-config('index_card_style') == 'multicard'
  @import './_index_card_style/multicard'
```

在 `_config.butterfly.yml` 中，添加以下内容  

```yml
# 主页卡片样式
# Docs: https://akilar.top/posts/d6b69c49/
index_card_style: multicard # slidecard | multicard
```

修改 `_config.yml` 的 `index_generator` 属性  

```yml
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 12
  order_by: -date
```

修改 `_config.butterfly.yml` 的 `index_post_content` 属性  

```yml
# Display the article introduction on homepage
# 1: description
# 2: both (if the description exists, it will show description, or show the auto_excerpt)
# 3: auto_excerpt (default)
# false: do not show the article introduction
index_post_content:
  method: 2
  length: 500 # if you set method to 2 or 3, the length need to config
```



#### 信息卡片头像状态

修改 `ThemeRoot/layout/includes/widget/card_author.pug` 的内容  

```diff
if theme.aside.card_author.enable
  .card-widget.card-info
    .is-center
-      .avatar-img
-        img(src=url_for(theme.avatar.img) onerror=`this.onerror=null;this.src='` + url_for(theme.error_img.flink) + `'` alt="avatar")
+      div.card-info-avatar
+        .avatar-img
+          img(src=url_for(theme.avatar.img) onerror=`this.onerror=null;this.src='` + url_for(theme.error_img.flink) + `'` alt="avatar")
+        div.author-status-box
+          div.author-status
+            g-emoji.g-emoji(alias="palm_tree" fallback-src="https://lskypro.acozycotage.net/LightPicture/2022/12/fe1dc0402e623096.jpg") 🐟
+            span 认真摸鱼中
```


在 `SiteSource/css/custom.css` 添加以下内容  

```css
.card-info-avatar .author-status-box {
  position: absolute;
  bottom: 0;
  left: calc(100% - 28px);
  width: 28px;
  height: 28px;
  border: 1px solid #d0d7de;
  border-radius: 2em;
  background-color: #f8f8f8f8;
  transition: 0.4s;
  overflow: hidden;
}

[data-theme="dark"] .card-info-avatar .author-status-box {
  background-color: #222222f2;
  border: 1px solid #5c6060;
}

.card-info-avatar .author-status-box .author-status {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 28px;
  padding: 0 5px;
}

.card-info-avatar .author-status-box:hover {
  width: 105px;
}

.card-info-avatar .author-status-box:hover .author-status span {
  width: 105px;
  margin-left: 4px;
}

.card-info-avatar .author-status-box .author-status span {
  width: 0;
  font-size: 12px;
  height: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  transition: 0.4s;
}

.card-widget .card-info-avatar {
  display: inline-block;
  position: relative;
}
```


#### 信息卡片背景图

在 `SiteSource/css/custom.css` 中添加以下内容  
> 可自行更换，一个用于 light，一个用于 dark  

```css
/* 个人信息卡片背景图 */
[data-theme="light"] #aside-content > .card-widget.card-info {
  background-image: url(https://pichost.threearapig.cc/wallpapers/light.webp);
  background-repeat: no-repeat;
  background-attachment: inherit;
  background-size: 100%;
}
[data-theme="dark"] #aside-content > .card-widget.card-info {
  background-image: url(https://pichost.threearapig.cc/wallpapers/dark.webp);
  background-repeat: no-repeat;
  background-attachment: inherit;
  background-size: 100%;
}
```


#### 头像呼吸灯

在 `SiteSource/css/custom.css` 中，添加以下内容  

```css
/* 头像呼吸灯 */
[data-theme="light"] .avatar-img {
  animation: huxi_light 4s ease-in-out infinite;
}
[data-theme="dark"] .avatar-img {
  animation: huxi_dark 4s ease-in-out infinite;
}
@keyframes huxi_light {
  0% {
    box-shadow: 0px 0px 1px 1px #e9f5fa;
  }
  50% {
    box-shadow: 0px 0px 5px 5px #e9f5fa;
  }
  100% {
    box-shadow: 0px 0px 1px 1px #e9f5fa;
  }
}
@keyframes huxi_dark {
  0% {
    box-shadow: 0px 0px 1px 1px #39c5bb;
  }
  50% {
    box-shadow: 0px 0px 5px 5px #39c5bb;
  }
  100% {
    box-shadow: 0px 0px 1px 1px #39c5bb;
  }
}
```


#### 字数统计显示 w 而不是 k

安装相应插件  

```bash
npm i hexo-wordcount-fomal --save
```

修改 `_config.butterfly.yml` 的 `workcount` 属性  

```yml
wordcount:
  enable: true
  post_wordcount: true
  min2read: true
  total_wordcount: true
```

#### 搜索  

**Algolia**  

在 `SiteRoot` 下执行以下命令  

```bash
npm install hexo-algoliasearch --save
```

修改 `_config.yml`，添加以下内容  

```yml
algolia:
  appId: "your applicationID"
  apiKey: "your Search-Only API Key"
  adminApiKey: "your Admin API Key"
  chunkSize: 5000
  indexName: "your indexName"
  fields:
    - content:strip:truncate,0,500
    - excerpt:strip
    - gallery
    - permalink
    - photos
    - slug
    - tags
    - title
```

> 需要去 algolia 官网注册，拿到填写的 `applicationID`、`Search-Only API Key`、`Admin API Key` 和 `indexName`  
> 其中 `indexName` 为创建时自行取的 `index`  


在 `SiteRoot` 下执行以下命令  

```bash
hexo algolia
```



修改 `_config.butterfly.yml` 的 `algolia_search` 属性  

```yml
algolia_search:
  enable: true
  hits:
    per_page: 10
  labels:
    input_placeholder: Search for Posts
    hits_empty: "我们没有找到任何搜索结果: ${query}"
    hits_stats: "找到${hits}条结果（用时${time} ms）"
```

