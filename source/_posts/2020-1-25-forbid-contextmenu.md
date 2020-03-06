---
layout: post
title: 全局禁止鼠标右键菜单
date: 2020-01-25 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
---

```javascript
$(document).bind("contextmenu",function(e){
    return false;
});	
$(document).bind("selectstart",function(e){
    return false;
});
```