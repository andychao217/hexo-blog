---
layout: post
title: 如何只在IE上加载CSS样式表
date: 2020-01-23 12:00:00
categories:
    - CSS
tags:
    - CSS
    - 前端
    - IE
---

## IE9以及低于IE9版本 :

可以使用条件注释语句来加载特定于ie的样式表。如下所示，使用外部样式表。

```HTML
<!--[if IE]>
  <link rel="stylesheet" type="text/css" href="all-ie-only.css" />
<![endif]-->
```

## IE10或IE11:

使用媒体查询（-ms-high-contrast）来加载样式表。由于-ms-high-contrast是微软特有的(并且只在IE 10+中可用)，所以只能在Internet Explorer 10或更高版本中解析。


```CSS
@media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
     /* IE10+ CSS styles go here */
}
```

## 微软 Edge12 :
可以使用@supports

```CSS
@supports (-ms-accelerator:true) {
  /* IE Edge 12+ CSS styles go here */
}
```

## 总结

如果我们想只针对IE加载样式表，只需要设置条件注释和-ms-high-contrast媒体查询即可。