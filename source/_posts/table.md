---
title: table
categories: [table]
tags: [table]
date: 2016-05-07 17:28:14
updated:
---

# table
表格是由行和列组成的结构化数据集(表格数据)，它能够使你简捷迅速地查找某个表示不同类型数据之间的某种关系的值 。

## `<tr>` 、`<th>` 和 `<td>`
`<tr>` 用于包裹 `<th>` 和 `<td>` 。

`<th>` 和 `<td>` 的属性
* colspan="number"  
col列,占多列,水平方向变成多格。

* rowspan="number"  
row行，占多行,	垂直方向变成多格。

## `<col>和<colgroup>`
`<col>` 元素被规定包含在 `<colgroup>` 容器中,用 `<col>` 来定义列的样式。

`<col>` 的属性
* span="number"  
用于将该 `<col>` 的样式用于
表格的第几列。
    ```
    <table>
        <colgroup>
            <col></col>
            <col style="background:red;" span="2"></col>        /*表格第二列变红*/
        </colgroup>
        <tr>
            <td>1</td>
            <td>2</td>
        </tr>
        <tr>
            <td>1</td>
            <td>2</td>
        </tr>
    </table>
    ```

## `<caption>`
给表格增加一个标题，放在 `<table>` 之间，与 `<tr>` 同级。

## `<thead>` 、`<tfoot>` 和 `<tbody>`
`<thead>` 需要嵌套在 `<table>` 中，放置在头部的位置。

`<tfoot>` 需要嵌套在 `<table>` 中，放置在底部 (页脚)的位置。即使放在 `<thead>` 的下面,浏览器仍将它呈现在表格的底部。

`<tbody>` 需要嵌套在 table 元素中。

## 表格单线
```
<style>
table{
    border: 1px solid red;
    border-collapse: collapse;      /*边框合并，表格的两边框合并为一条*/
}
</style>
<table border="" cellspacing="0" cellpadding=""></table>
```

## 模拟table
table表格中的单元格最大的特点之一就是同一行列表元素都等高。所以，很多时候，我们需要等高布局的时候，就可以借助display:table-cell属性。

`display: table;`模拟 `<table>`

`display: table-row;` 模拟 `<tr>`

`display: table-cell;` 模拟 `<td>`

```
<style>
.table{
    display: table;
    width: 100px;
}
.tr{
    display: table-row;
}
.td{
    display: table-cell;
}
</style>
<div class="table">
    <div class="tr">
        <div class="td">1</div>
        <div class="td">2</div>
        <div class="td">2</div>
    </div>
    <div class="tr">
        <div class="td">1</div>
        <div class="td">2</div>
        <div class="td">2</div>
    </div>
</div>
```

## 实现垂直居中
元素使用 `display:table-cell;
        vertical-align:middle;
        text-align:center;`
        可以使内部的文字，图片水平垂直居中(块级元素不行)。
        



