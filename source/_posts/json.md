---
title: JSON 数据交换格式
categories: [JSON]
tags: [JSON]
date: 2016-11-03 11:25:05
updated:
---

# JSON 数据交换格式
 JavaScript Object Notation (JSON) 是一种数据交换格式。尽管不是严格意义上的子集，JSON 非常接近  JavaScript 语法的子集(只是接近，但不是子集). 虽然许多编程语言支持 JSON，JSON 对于基于 JavaScript 的应用尤其常用（包括网站和浏览器扩展）。
 
 JSON 可以表示数字、布尔值、字符串、null、数组（值的有序序列），以及由这些值（或数组、对象）所组成的对象（字符串与值的映射）。JSON 并非原生支持更复杂的数据类型（如函数、正则表达式、日期等）。
 
## JavaScript 与 JSON 的区别
 

JavaScript类型|JSON 区别
-|-
对象和数组|属性名称必须是双引号字符串；后缀逗号被禁止。最后一个属性后面不能有逗号。
数值|前导0是禁止的（在JSON.stringify 0将被忽略，但在JSON.parse它将抛出SyntaxError）；小数点后必须至少有一位数字。
字符串|只有有限的一些字符可能会被转义；禁止某些控制字符; Unicode 行分隔符 (U+2028) 和段分隔符 (U+2029)被允许 ; 字符串必须是双引号。请参见以下示例，其中 JSON.parse() 能够正常解析，但把它当作JavaScript解析时会抛出 SyntaxError 错误。
   

## text/json和application/json之间的区别  
application/json是json的官方MIME类型。  
text/json是在application/json得到官方认定之前的非官方MIME类型。