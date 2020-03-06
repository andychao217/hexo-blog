---
layout: post
title: Meta name解释
date: 2020-02-05 12:00:00
categories:
    - HTML
tags:
    - HTML
    - Meta
---

# meta name解释

```HTML
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```
_content属性值_
```css
width:可视区域的宽度，值可为数字或关键词device-width
height:同width
intial-scale:页面首次被显示是可视区域的缩放级别，取值1.0则页面按实际尺寸显示，无任何缩放
maximum-scale=1.0, minimum-scale=1.0;可视区域的缩放级别，
maximum-scale用户可将页面放大的程序，1.0将禁止用户放大到实际尺寸之上。
user-scalable:是否可对页面进行缩放，no 禁止缩放
``` 
```HTML
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
```
Edge 模式告诉 IE 以最高级模式渲染文档，也就是任何 IE 版本都以当前版本所支持的最高级标准模式渲染，避免版本升级造成的影响。简单的说，就是什么版本 IE 就用什么版本的标准模式渲染
```HTML
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```
使用以下代码强制 IE 使用 Chrome Frame 渲染
```HTML
<meta http-equiv="X-UA-Compatible" content="chrome=1">
``` 
最佳的兼容模式方案，结合考虑以上两种：
```HTML
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
``` 