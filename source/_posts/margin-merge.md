---
title: 外边距合并
categories: [CSS]
tags: [margin]
date: 2016-06-19 14:23:39
updated:
---

# 外边距合并
块的顶部外边距和底部外边距有时被组合(折叠)为单个外边距，其大小是组合到其中的最大外边距，这种行为称为外边距塌陷(margin collapsing)，有的地方翻译为外边距合并。

出现外边距合并的三种情况:
* 同级的相邻元素  
同级相邻的两个元素元素元素之间的外边距会塌陷。
    ```
    <div style="margin-bottom:20px;border-bottom:1px solid red;">1111</div>  /*这两个元素的上下间距是 20px ,而不是相加的 40px */
    <div style="margin-top:20px;border-top:1px solid red;">2222</div>
    ```

* 块级父元素与其第一个/最后一个子元素   
如果块级父元素中，不存在上边框(border-top)、上内边距(padding-top)、内联元素(该元素在父元素和第一个子元素之间)、 清除浮动(clear,overflow) 这四条属性（也可以说，当上边框宽度及上内边距距离为0时），那么块级父元素和其第一个子元素就会发生上外边距合并的现象，此时这个父元素对外展现出来的外边距将直接变成这个父元素和其第一个子元素的margin-top的较大者。
    ```
    <style>
    .box{
        background: red;
    }
    .son{
        margin-top: 20px;
    }
    </style>
    <hr />
    <div class="box">       /*此时与上面的 <hr /> 多了 20px 间距，但是该元素本身没设置 margin ，这里多的 margin 来源于子级*/
        <div class="son">1111</div>
    </div>
    ```
    ![](https://ws2.sinaimg.cn/large/006tNc79ly1flhl96wjggj305u05pjra.jpg)

    类似的，若块级父元素的 margin-bottom 与它的最后一个子元素的margin-bottom 之间没有父元素的 border、padding、inline content、height、min-height、 max-height 分隔时，就会发生 下外边距合并 现象。
    ```
    .box{
        background: red;
    }
    .son{
        margin-bottom: 20px;
    }
    </style>
    <div class="box">       /*此时与下面的 <hr /> 多了 20px 间距，但是该元素本身没设置 margin ，这里多的 margin 来源于子级*/
        <div class="son">1111</div>
    </div>
    <hr />    
    ```
    ![](https://ws3.sinaimg.cn/large/006tNc79ly1flhlcphbjqj303602ia9x.jpg)

* 空块元素  
如果存在一个空的块级元素，其 border、padding、inline content、height、min-height 都不存在。那么此时它的上下边距中间将没有任何阻隔，此时它的上下外边距将会合并。
    ```
    <div style="margin-bottom: 0px;">1111</div>     /*第一个元素和第三个元素之间的间距是 20px ,而不是 40px 。*/
    <div style="margin-top: 20px; margin-bottom: 20px;"></div>
    <div style="margin-top: 0px;">2222</div>
    ```
