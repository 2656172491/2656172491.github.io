---
title: 将butterfly主题的背景设为透明
tags: hexo美化
summary: >-
  这里是言寺AI，这篇文章介绍了如何将Butterfly主题的博客背景设置为透明，作者因觉得白色背景刺眼而参考网友方案进行修改，首先安装cheerio依赖用于解析HTML，接着修改主题配置文件_config.butterfly.yml，设置index_img、footer_img和background等参数，然后在butterfly/source/css目录下创建modify.styl文件编写CSS样式，对页面顶部图、页脚等元素进行透明化处理并适配不同屏幕尺寸，之后在主题配置中通过inject引入该样式文件，最后在butterfly/scripts目录创建modify.js文件，利用cheerio和hexo扩展功能编写JavaScript代码动态插入顶部图并修改HTML结构，从而完成整个页面背景透明化的美化过程。
date: 2025-01-19 23:23:11
---

# 介绍

在对博客页面进行一定美化后，觉得白色的背景色有点难受，于是借鉴网友的修改方案进行修改，将背景色改为透明。

借鉴文档：[Butterfly 主题一图流背景与顶部图修改 | 边缘矩阵](https://android99.com/p/butterfly-top-image-modify/#c1d4d87a625763710038483172232cf3)

已将大佬 [边缘矩阵](https://android99.com/) 放在友联中

# 具体配置过程

## 增加依赖

因为配置的过程中会涉及到使用cheerio来对html进行解析，所以要安装一下依赖

```
npm install cheerio
```



## 主题配置文件修改

首先是修改“_config.butterfly.yml“的内容

```
# --------------------------------------
# Image Settings
# --------------------------------------

# The banner image of index page
index_img: transparent

# The background image of footer
footer_img: transparent

# Website Background
# Can set it to color or image url
background: [图片路径]

# --------------------------------------
# Index page settings
# --------------------------------------

# The height of top_img, eg: 300px/300em/300rem
index_top_img_height: 400px

```

## css样式修改

在butterfly/source/css目录下创建一个新的文件，命名为”modify.styl“ 内容如下

```
@import 'nib'

// 顶部图
#page-header
  background: transparent !important

  &.post-bg, &.not-home-page
    height: 280px !important
  #post-info
    bottom: 40px !important
    text-align: center
  #page-site-info
    top: 140px !important

  @media screen and (max-width: 768px)
    &.not-home-page
      height: 200px !important
    #post-info
      bottom: 10px !important
    #page-site-info
      top: 100px !important

.top-img
  height: 250px
  margin: -50px -40px 50px
  border-top-left-radius: inherit
  border-top-right-radius: inherit
  background-position: center
  background-size: cover
  transition: all .3s

  @media screen and (max-width: 768px)
    height: 230px
    margin: -36px -14px 36px

  [data-theme='dark'] &
    filter: brightness(.8)

// 页脚
#footer:before
  background-color: alpha(#FFF, .5)

  [data-theme='dark'] &
    background-color: alpha(#000, .5)

#footer-wrap, #footer-wrap a
  color: #111
  transition: unset

  [data-theme='dark'] &
    color: var(--light-grey)
```

## 引入样式

```
# --------------------------------------
# Other Settings
# --------------------------------------

# Inject
# Insert the code to head (before '</head>' tag) and the bottom (before '</body>' tag)
inject:
  head:
    - <link rel="stylesheet" href="/css/modify.css">
```

## js文件配置

在butterfly/scripts目录中创建modify.js文件，并添加以下内容

```
'use strict';
const { filter } = hexo.extend;
const cheerio = require('cheerio');

/**
 * 在页面插入新顶部图
 * @param {cheerio.Root} $ Root
 */
function insertTopImg($) {
    const header = $('#page-header');
    if (header.length === 0) return;
    const background = header.css('background-image');
    if (!background) return;
    $('#post, #page, #archive, #tag, #category').prepend(`<div class="top-img" style="background-image: ${background};"></div>`);
}

// 修改 HTML
filter.register('after_render:html', (str, data) => {
    const $ = cheerio.load(str, {
        decodeEntities: false
    });
    insertTopImg($);
    return $.html();
});
```

至此 关于页面背景的美化已经完成了