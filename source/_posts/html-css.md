---
title: HTML ❤️ CSS
date: 2016-03-09 22:56:09
tags:
- HTML
- CSS
---

>一句话简单概括，HTML是五官，CSS是颜值，JS是表情

<iframe src="//slides.com/kalasoo/xitu-html-css-tutorial/embed?style=light" width="100%" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

<!-- more -->

## 自定义 Hexo 主题

当学习了一些HTML、CSS的基础知识之后，我们就可以来自定义我们的Hexo主题了。先看一下 hexo 自带的默认主题 landscape 的目录结构。

```
|-- Gruntfile.js
|-- LICENSE
|-- README.md
|-- _config.yml
|-- languages
|-- layout
|   |-- _partial
|   |-- _widget
|   |-- archive.ejs
|   |-- category.ejs
|   |-- index.ejs
|   |-- layout.ejs
|   |-- page.ejs
|   |-- post.ejs
|   `-- tag.ejs
|-- package.json
|-- scripts
|   `-- fancybox.js
`-- source
    |-- css
    |-- fancybox
    `-- js
```

**_config.yml** 主题的配置文件。修改时会自动更新，无需重启服务器
**languages** 语言文件夹。用来进行国际化。
**layout** 布局文件夹。
**scripts** 脚本文件夹。在启动时，Hexo 会载入此文件夹内的 JavaScript 文件。
**source** 资源文件夹，除了模板以外的 Asset，例如 CSS、JavaScript 文件等，都应该放在这个文件夹中。文件或文件夹开头名称为 _（下划线线）或隐藏的文件会被忽略。

如果文件可以被渲染的话，会经过解析然后储存到 public 文件夹，否则会直接拷贝到 public 文件夹。

### 修改主题的配色

明白了每个目录下文件的作用之后，自己就可以进行主题的修改了。先来最简单的主题配色修改。
进入 `source/css` 目录， 打开 `_variables.styl` 文件。修改起来应该很简单了，只要修改颜色值即可。

```
// Config
support-for-ie = false
vendor-prefixes = webkit moz ms official

// Colors
color-default = #555
color-grey = #999
color-border = #ddd
color-link = #258fb8
color-background = #eee
color-sidebar-text = #777
color-widget-background = #ddd
color-widget-border = #ccc
color-footer-background = #262a30
color-mobile-nav-background = #191919
color-twitter = #00aced
color-facebook = #3b5998
color-pinterest = #cb2027
color-google = #dd4b39

// Fonts
font-sans = "Helvetica Neue", Helvetica, Arial, sans-serif
font-serif = Georgia, "Times New Roman", serif
font-mono = "Source Code Pro", Consolas, Monaco, Menlo, Consolas, monospace
font-icon = FontAwesome
font-icon-path = "fonts/fontawesome-webfont"
font-icon-version = "4.0.3"
font-size = 14px
line-height = 1.6em
line-height-title = 1.1em

// Header
logo-size = 40px
subtitle-size = 16px
banner-height = 300px
banner-url = "images/banner.jpg"

sidebar = hexo-config("sidebar")

// Layout
block-margin = 50px
article-padding = 20px
mobile-nav-width = 280px
main-column = 9
sidebar-column = 3

if sidebar and sidebar isnt bottom
  _sidebar-column = sidebar-column
else
  _sidebar-column = 0

// Grids
column-width = 80px
gutter-width = 20px
columns = main-column + _sidebar-column

// Media queries
mq-mobile = "screen and (max-width: 479px)"
mq-tablet = "screen and (min-width: 480px) and (max-width: 767px)"
mq-normal = "screen and (min-width: 768px)"
```

### 修改主题的布局

**layout** 布局文件夹。用于存放主题的模板文件，决定了网站内容的呈现方式，Hexo 内建 Swig 模板引擎，您可以另外安装插件来获得 EJS、Haml 或 Jade 支持，Hexo 根据模板文件的扩展名来决定所使用的模板引擎，例如：
`layout.ejs   - 使用 EJS`
`layout.swig  - 使用 Swig`
可以看出 landscape 主题使用的 EJS 模板引擎。

进入 `layout/_partial` 目录，网页的各个部分的布局文件都在这里。

```
|-- after-footer.ejs
|-- archive-post.ejs
|-- archive.ejs
|-- article.ejs
|-- footer.ejs
|-- google-analytics.ejs
|-- head.ejs
|-- header.ejs
|-- mobile-nav.ejs
|-- post
|   |-- category.ejs
|   |-- date.ejs
|   |-- gallery.ejs
|   |-- nav.ejs
|   |-- tag.ejs
|   `-- title.ejs
`-- sidebar.ejs

```

代开 `footer.ejs` 文件，修改一下网页的页脚信息。

```
<footer id="footer">
  <% if (theme.sidebar === 'bottom'){ %>
    <%- partial('_partial/sidebar') %>
  <% } %>
  <div class="outer">
    <div id="footer-info" class="inner">
      &copy; <%= date(new Date(), 'YYYY') %> <%= config.author || config.title %><br>
      <%= __('powered_by') %> <a href="http://hexo.io/" target="_blank">Hexo</a>
    </div>
    <!-- 添加网页信息 -->
    <div>xitu.io</div>
  </div>
</footer>
```

### 修改主题的 JavaScript 脚本

我们可以自定义一些 `JavaScript` 脚本放到 `source/js` 目录下，来定义网页的行为。比如加入图片轮播。
或者放到 `scripts` 目录下，在启动时，Hexo 会载入此文件夹内的 JavaScript 文件。


扩展阅读：
[GRUNT - JavaScript 世界的构建工具](http://www.gruntjs.net/getting-started)
[Hexo 主题](https://hexo.io/zh-cn/docs/themes.html)
[ejs - Embedded JavaScript templates](https://www.npmjs.com/package/ejs)


## LEARN MORE

**Tutorials**
- http://learn.shayhowe.com/html-css
- http://learn.shayhowe.com/advanced-html-css
 
**Docs**
- https://developer.mozilla.org
- https://www.w3.org
 
**Keep Updated**
- https://css-tricks.com
- http://tympanus.net/codrops
- http://www.html5rocks.com