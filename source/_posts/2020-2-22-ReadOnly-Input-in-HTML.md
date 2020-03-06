---
layout: post
title: HTML中让表单Input等文本框为只读不可编辑的方法
date: 2020-02-22 12:00:00
categories:
    - HTML
tags:
    - HTML
    - 前端
---

## HTML中让表单input等文本框为只读不可编辑的方法
有时候，我们希望表单中的文本框是只读的，让用户不能修改其中的信息，实现的方式归纳一下，有如下几种。 

方法1: onfocus=this.blur() 当鼠标放不上就离开焦点
```HTML
<input type="text" name="input1" value="中国" onfocus=this.blur()> 
```
--------------
方法2:readonly
```HTML
<input type="text" name="input1" value="中国" readonly> 
<input type="text" name="input1" value="中国" readonly="true"> 
```
--------------
方法3: disabled
```HTML
<input type="text" name="input1" value="中国" disabled="true">
```
--------------
完整的例子:
```HTML
<input name="ly_qq" type="text" tabindex="2"
onMouseOver="this.className='input_1'" 
onMouseOut="this.className='input_2'" 
value="123456789" disabled="true"　readOnly="true" /> 
```
+ disabled="true" 文字会变成灰色，不可编辑。 
+ readOnly="true" 文字不会变色，也是不可编辑的
--------------
```HTML
css屏蔽输入：<input style="ime-mode: disabled"> 
```
--------------
+ 第一：disabled="disabled"这样定义之后被禁用的 input 元素既不可用，也不可点击。
+ 第二：readonly="readonly" 只读字段是不能修改的。不过，用户仍然可以使用 tab 键切换到该字段，还可以选中或拷贝其文本；