---
title: 垂直居中
categories: [CSS]
tags: [CSS3,flex,table]
date: 2016-06-14 11:02:35
updated:
---

# 垂直居中

## flex (子级宽高未知)
父级增加属性 `display: flex;` 、`justify-content: center;` 、`align-items: center;`
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid red;
        display: flex;      
        justify-content: center;
        align-items: center;
    }
</style>
<div class="box">
    <div class="item">1</div>
</div>
```

## 定位 + CSS3 (子级宽高未知)
父级 `position: relative;` ,子级 `position: absolute;` 、`top: 50%;` 、`left: 50%;`、`transform: translate(-50%,-50%);`
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid red;
        position: relative;
    }
    .item{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
</style>
<div class="box">
    <div class="item">1</div>
</div>
```

## 定位 (子级宽高未知)
父级 `position: relative;` ,子级 `position: absolute;` 、`top: 0;` 、`bottom: 0;` 、`left: 0;` 、`right: 0;` 、`margin: auto;`
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid red;
        position: relative;
    }
   .item{
        width: 50px;
        height: 50px;
        background: red;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
    }
</style>
<div class="box">
    <div class="item">1</div>
</div>
```

## 定位 (子级宽高已知)
父级 `position: relative;` ,子级 `position: absolute;` 、`top: 50%;` 、`left: 50%;`、`margin-top: -高一半` 、`margin-left: -宽一半`
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid red;
        position: relative;
    }
    .item{
        position: absolute;
        width: 50px;
        height: 50px;
        top: 50%;
        left: 50%;
        margin-left: -25px;
        margin-top: -25px;
    }
</style>
<div class="box">
    <div class="item">1</div>
</div>
```

## table-cell (子级宽高未知)
父级 `display: table-cell;` 、`text-align: center;` 、`vertical-align: middle;` ，子级 `display: inline-block;`
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid red;
        display: table-cell;
        text-align: center;
        vertical-align: middle;
    }
    .item{
        width: 50px;
        height: 50px;
        background: red;
        display: inline-block;
    }
</style>
<div class="box">
    <div class="item">1</div>
</div>
```
或者  

父级 `display: table-cell;` 、`vertical-align: middle;` ，子级 `display: block;`、`margin: 0 auto;`
```
<style>
    .box {
        width: 300px;
        height: 300px;
        border: 1px solid red;
        display: table-cell;
        vertical-align: middle;
    }
    .item{
        width: 50px;
        height: 50px;
        background: red;
        display: block;
        margin: 0 auto;
    }
</style>
<div class="box">
    <div class="item">1</div>
</div>
```

