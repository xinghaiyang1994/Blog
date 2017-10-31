---
title: Smarty 语法
date: 2017-10-31 15:04:33
tags: [smarty]
categories: PHP
---

# Smarty 语法
* 子模板继承
    ```
    {{extends file="./_meta.tpl"}}      /*必须放在第一行*/
    ```

* 赋值
    ```
    {{assign var="aaa" value="777"}}
    <h1>{{$aaa}}</h1>       /*777*/
    
    {{$qqq="222"}}
    <h1>{{$qqq}}</h1>       /*222*/
    ```

* 循环
    * for循环
        ```
        {{for $i= 1 to 10}}
            <h3>{{$i}}</h3>
        {{/for}}
        ```
    
    * foreach循环
        ```
        {{foreach from=$arr item=list}}
            <h3>{{$list}}</h3>
        {{/foreach}}
        
        /*或者*/
        {{foreach $list as $arr}}
            <h3>{{$list}}</h3>
        {{/foreach}}
        
        ```
        {foreach}的属性:
        * @index  
        下标，从0开始   
        
        * @iteration  
        下标,从1开始
        
        * @first  
        第一个

        * @last  
        最后一个

        * @show  
        循环执行之后， 检测循环是否显示数据的判断。
        
        * @total  
        总次数
        
* 判断
    ```
	{{foreach $arr as $item}}
        {{if $arr@index == 3}}
        	<h5>999</h5>	
        {{elseif $arr@index ==1 }}
        	<h4>这是11111</h4>
        {{else}}
        	<h1>没有999</h1>
        {{/if}}
        <h3>{{$arr@index}}</h3>
	{{/foreach}}
    ```
        
* 载入模板  
`{{include}}`用于载入其他模板到当前模板中。 在包含模板中可用的变量，载入后在当前模板仍然可用。
    ```
    {{block name="view"}}
        {{include file="./components/first/first.tpl"}}
    {{/block}}
    ```
