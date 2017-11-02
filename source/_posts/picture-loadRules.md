---
title: 图片加载与渲染规则
categories: [HTML]
tags: [img]
date: 2017-11-02 11:27:05
updated:
---

# 图片加载与渲染规则
页面中不是所有的<img>标签图片和样式表背景图片都会加载。
## display:none
* 设置了display:none属性的元素，图片不会渲染出来，但会加载(包括img标签、标签的background-image)。 


    <style>
    .img-purple {
        background-image: url(../image/purple.png); /*不会渲染，会加载*/
    }
    </style>
    <img src="../image/pink.png" style="display:none"> <!--不会渲染，会加载-->
    <div class="img-purple" style="display:none"></div>
    
原理：  
>把DOM树和样式规则树匹配构建渲染树时，会把可渲染元素上的所有属性（如display:none属性和background-image属性）结合一起产出到渲染树。   
>
>当解析渲染树时会加载<img>标签元素上的图片，发现元素上有background-image属性时会加载背景图片。 
>
>当绘制时发现元素上有display:none属性，则不计算该元素位置，也不会绘制该元素。  

* 设置了display:none属性元素的子元素，样式表中的背景图片不会渲染出来，也不会加载；而<img>标签的图片不会渲染出来，但会加载。              


    <style>
    .img-yellow {
        background-image: url(../image/yellow.png); /*不会渲染，不会加载*/
    }  
    </style>
    <div style="display:none">
        <img src="../image/red.png"> <!--不会渲染，会加载-->
        <div class="img-yellow"></div>
    </div>

原理:
>正如上面所说的，构建渲染树时，只会把可渲染元素产出到渲染树，这就意味有不可渲染元素，当匹配DOM树和样式规则树时，若发现一个元素的属性上有display:none，浏览器会认为该元素的子元素是不可渲染的，因此不会把该元素的子元素产出到渲染树上。 

## 重复图片
* 页面中多个<img>标签或样式表中的背景图片图片路径是同一个，图片只加载一次。


原理:
>浏览器请求资源时，都会先判断是否有缓存，若有缓存且未过期则会从缓存中读取，不会再次请求。先加载的图片会存储到浏览器缓存中，后面再次请求同路径图片时会直接读取缓存中的图片。

## 不存在元素的背景图片
* 不存在元素的背景图片不会加载。


    .img-blue {
        background-image: url(../image/blue.png); /*没有元素有该类名，不会加载*/
    }
    .img-orange{
        background-image: url(../image/orange.png); /*会加载*/
    }
    <div class="img-orange"></div>
    
原理:
>不存在的元素不会产出到DOM树上，因此渲染树上也不会有不存在的元素，当解析渲染树时无法解析不存在的元素，不存在的元素上的图片自然不会加载也不会渲染。

## 伪类的背景图片
* 当触发伪类的时候，伪类样式上的背景图片才会加载。

>触发hover前，DOM树与样式规则树匹配的是无hover状态选择器.img-green的样式，因此渲染树上background-image属性的值是url(../image/green.png)，解析渲染树时加载的是green.png，绘制时渲染的也是green.png。
>
>触发hover后，因为.img-green:hover的优先级比较高，因此DOM树与样式规则树匹配的是有hover状态选择器.img-green:hover的样式，渲染树上background-image属性的值是url(../image/red.png)，解析渲染树时加载的是red.png，绘制时渲染的也是red.png。

# 应用
* 占位图  
当使用样式表中的背景图片作为占位符时，要把背景图片转为base64格式。这是因为背景图片加载的顺序在<img>标签后面，背景图片可能会在<img>标签图片加载完成后才开始加载，达不到想要的效果。

* 预加载  
很多场景里图片是在改变或触发状态后才显示出来的，例如点击一个Tab后，一个设置display:none隐藏的父元素变为显示，这个父元素里的子元素图片会在父元素显示后才开始加载；又如当鼠标hover到图标后，改变图标图片，图片会在hover上去后才开始加载，导致出现闪一下这种不友好的体验。
在这种场景下，我们就需要把图片预加载，预加载有很多种方式:   
1.若是小图标，可以合并成雪碧图，在改变状态前就把所有图标都一起加载了。  
2.使用上文讲到的，设置了display:none属性的元素，图片不会渲染出来，但会加载。把要预加载的图片加到设置了display:none的元素背景图或<img>标签里。  
3.在javascript创建img对象，把图片url设置到img对象的src属性里。














