---
title: 文字效果
categories: [CSS]
tags: [布局]
date: 2016-5-15 17:50:53
updated:
---

# 文字效果

## 文字环绕
图片与文字同级，图片浮动，文字不浮动，父级清浮动。

```html
<style>
    .box{
        overflow: hidden;
        width: 300px;
    }
    img{
        float: left;
    }
</style>
<div class="box">
    <img src="https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo_top_ca79a146.png" alt="" class="fr" width="100">
    <div>浮动通常用来创建整个网站布局，其中包括浮动的多列信息，因此它们彼此并排放置（默认行为是列彼此之间的排列顺序与它们在源中显示的顺序相同）。虽然有更新的更好的布局技术可用，但我们将在本模块的后面探讨，浮动仍然是最受喜欢的老物，因为它们可以支持到 Internet Explorer 4。</div>
</div>
```

![](https://ws1.sinaimg.cn/large/006tKfTcly1fld5m6j3azj308t087gnc.jpg)

## 首字下沉
元素伪类首字浮动。

```html
<style>
    .box{
        width: 300px;
    }
    .box::first-letter{
       float: left;
       font-size: 2em;
       padding: 10px;
    }
</style>
<div class="box">浮动通常用来创建整个网站布局，其中包括浮动的多列信息，因此它们彼此并排放置（默认行为是列彼此之间的排列顺序与它们在源中显示的顺序相同）。虽然有更新的更好的布局技术可用，但我们将在本模块的后面探讨，浮动仍然是最受喜欢的老物，因为它们可以支持到 Internet Explorer 4。</div>
```

![](https://ws2.sinaimg.cn/large/006tKfTcly1fld5w1o7i9j308t07ztab.jpg)

