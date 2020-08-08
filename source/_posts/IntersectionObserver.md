---
title: IntersectionObserver
categories: []
tags: []
date: 2019-08-08 22:33:37
updated:
---

# IntersectionObserver
IntersectionObserver 接口提供了一种异步观察目标元素与其祖先元素或顶级文档视窗(viewport)交叉状态的方法。

兼容性：IE 不支持，safari 12.1才开始支持

```js
const observer = new IntersectionObserver(callback, options?);
```
参数说明：
* callback  
    `(entries, observer) => {}`，当元素可见比例超过指定阈值后，会调用一个回调函数。  
    回调函数的参数：
    * entries  
        一个 IntersectionObserverEntry 对象的数组，每个被触发的阈值，都或多或少与指定阈值有偏差。
    * observer  
        被调用的 IntersectionObserver 实例。
* options  
    配置 observer 实例的对象。  
    如果options未指定，observer实例默认使用文档视口作为root，并且没有margin，阈值为0%（意味着即使一像素的改变都会触发回调函数）。  
    配置项：
    * root  
        监听元素的祖先元素Element对象，其边界盒将被视作视口。目标在根的可见区域的的任何不可见部分都会被视为不可见。
    * rootMargin  
        一个在计算交叉值时添加至根的边界盒中的一组偏移量，类型为字符串(string) ，可以有效的缩小或扩大根的判定范围从而满足计算需要。语法大致和CSS 中的 margin 属性等同。  
        默认值是 `'0px 0px 0px 0px'`。
    * threshold  
        规定了一个监听目标与边界盒交叉区域的比例值，可以是一个具体的数值或是一组 0 到 1 之间的数组。若指定值为 0 ，则意味着监听元素即使与根有 1 像素交叉，此元素也会被视为可见. 若指定值为 1，则意味着整个元素都是可见的。  
        默认值为 `[0]`
* 返回值  
    一个可以使用规定阈值监听目标元素可见部分与 root 交叉状况的新的 IntersectionObserver 实例。

回调函数中 IntersectionObserverEntry 实例的属性：
* intersectionRatio    
    目标元素的可见比例，即 intersectionRect 占 boundingClientRect 的比例，完全可见时为 1 ，完全不可见时小于等于 0 。
* target  
    被观察的目标元素，是一个 DOM 节点对象。
* boundingClientRect  
    返回包含目标元素的边界信息的 DOMRectReadOnly. 边界的计算方式与  `Element.getBoundingClientRect()` 相同。
* intersectionRect  
    目标元素与视口（或根元素）的交叉区域的信息。
* rootBounds  
    根元素的矩形区域的信息，`getBoundingClientRect()` 方法的返回值，如果没有根元素（即直接相对于视口滚动），则返回 null 。
* time  
    可见性发生变化的时间，是一个高精度时间戳，单位为毫秒。

## observer 方法
* observe(targetElement)  
    被观察的元素。
* unobserve(targetElement)  
    停止对一个元素的观察。
* disconnect()  
    终止对所有目标元素可见性变化的观察。
* takeRecords()  
    返回一个 IntersectionObserverEntry 对象数组, 每个对象的目标元素都包含每次相交的信息, 可以显式通过调用此方法或隐式地通过观察者的回调自动调用。

## 入门示例
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>getBoundingClientRect</title>
  <style>
    *{
      padding: 0;
      margin: 0;
    }
    .parent{
      border: 1px solid red;
      width: 200px;
      height: 200px;
      overflow-x: auto;
    }
    .son{
      width: 100px;
      height: 100px;
      background: blue;
      margin-left: 300px;
    }
    .line{
      width: 800px;
      height: 1px;
    }
  </style>
</head>
<body>
  <div class="parent">
    <div class="line"></div>
    <div class="son">son</div>
  </div>
  <script>
    const oSon = document.querySelector('.son');
    var io = new IntersectionObserver((arr) => {
      let target = arr[0]
      if (target.intersectionRatio <= 0) {
        return
      }
      console.log('出现', target.intersectionRatio);
    });
    io.observe(oSon);
  </script>
</body>
</html>
```