---
layout: post
title: jQuery里面获取宽度,高度,x,y坐标
date: 2020-02-14 12:00:00
categories:
	- JavaScript
	- jQuery
tags:
    - JavaScript
    - 前端
    - jQuery
---

### jQuery里面获取div区块的宽度与高度

```javascript
获取宽度
$('div').width();             获取：区块的本身宽度
$('div').outerWidth();        获取：区块的宽度+padding宽度+border宽度
$('div').outerWidth(true);    获取：区块的宽度+padding宽度+border宽度+margin的宽度

获取高度
$('div').height();            获取：区块的本身高度
$('div').outerHeight();       获取：区块的高度+padding高度+border高度
$('div').outerHeight(true);   获取：区块的高度+padding高度+border高度+margin的高度
```
### JQuery 获得div绝对,相对位置的坐标方法


```javascript
获取页面某一元素的绝对X,Y坐标

var X = $('#DivID').offset().top;
var Y = $('#DivID').offset().left;

获取相对(父元素)位置

var X = $('#DivID').position().top;
var Y = $('#DivID').position().left;
```

```javascript
onmousemove="getCoords(event, this)"

function getCoords(ev,obj){
	var width = $('#scenes').width(); 
	var height = $('#scenes').height();
	var x = ev.clientX-obj.offsetLeft;
    var y = ev.clientY-obj.offsetTop;
	var coord_x = (x/width*100).toFixed(2) + '%';
	var coord_y = (y/height*100).toFixed(2)+ '%';
	document.getElementById('coord_x').value = coord_x;
	document.getElementById('coord_y').value = coord_y;
}
```
