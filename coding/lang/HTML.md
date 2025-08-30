# HTML
> 超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言

[toc]{type: "ol", level: [2,3]}

## 简介
### 实例解析
![Img](https://www.runoob.com/wp-content/uploads/2013/06/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg)
- <!DOCTYPE HTML>声明为HTML5文档
- \<html>是根元素
- \<head>是头部元素,包含元数据(meta),如
    - \<meta charset="utf-8">:定义网页编码格式
    - \<title>:文档标题
- \<body>元素包含可见的页面内容

<br/>

### 什么是HTML
HTML是用来描述网页的一种语言
- HTML指的是超广西标记语言:HyperText Markup Language
- HTML不是一种编程语言,是一种标记语言
- 标记语言是一套标记标签
- HTML文档包含HTML标签和文本内容
- 文件后缀一般为.html或.htm

<br/>

### HTML标签
HTML tag,与HTML元素同义
- HTML标签是由尖括号包围的关键词
- 通常是成对出现的,<tag></tag>
- 一般是使用小写字母

<br/>

### 标签属性
- HTML标签可以设置属性
- 属性可以在元素中添加附加信息
- 属性一般描述于开始标签
- 一般是以键值对的形式出现,如name=‘value’
- 常用属性如下表
| 属性 | 说明 |
| -- | -- | 
| class | TD |  
| id | TH |
| style | -- | 
| TD | TD |    
<br/>

## 标签
### 注释标签
```html
<!--注释内容-->

```
一般快捷键是`ctrl + /`
c#里是`ctrl + k + c`
<br/>

### div
最常用的标签
```html
<div></div>

```

<br/>

### 文本标签

### 
```html
<!-- 标题标签(Heading) -->
<h1>这是一级标题</h1>
<h1>这是二级标题</h1>
<h1>这是三级标题</h1>
<h1>这是四级标题</h1>
<h1>这是五级标题</h1>
<h1>这是六级标题</h1>

<!-- 段落标签 -->
<p></p>

<!-- text -->
<text></text>
<!-- span -->
<span></span>

<!-- 加粗Bold -->
<b></b>
<!-- 斜体Italic -->
<i></i>
<!-- 下划线 -->
<u></u>
<!-- 删除线 -->
<s></s>
```

<br/>

### 链接
```html
<a href='https://www.bing.com'>打开一个链接</a>
```

<br/>

### 图像
```html
<img src='' width='' height=''/>
```
可以使用width和height设定展示图片大小

### table
```html
<!-- 表格 -->
<table>
    <!-- 表头 -->
    <thead>
        <!-- 行 -->
        <tr>
            <!-- 表头单元格 -->
            <th></th>
        </tr>
    </thead>
    <!-- 表格主体 -->
    <tbody>
        <tr>
            <!-- 单元格 -->
            <td></td>
        </tr>
    </tbody>
    <tfoot>

    </tfoot>
</table>
```

### 列表

<br/>

### 交互组件
```html
<input/>

<select/>

<checkbox>

<button>按钮</button>
```
<br/>

### 页面元素引入组件
```html
<section></section>

<!-- 定义脚本 -->
<script></script>

<!-- 定义可使用模板 -->
<template></template>

<!-- 定义css样式 -->
<style></style>
```

<br/>

## 属性

## 布局

## 表单

## 事件

## 框架

