---
layout: post
title: iOS设备上iframe的宽度会扩大的解决办法
date: 2020-02-18 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - iframe
    - iOS
---

##### 解决办法，就是在iframe加载完成后，设置 iframe里面body的宽度为多少PX。


```javascript
$("#iframe1")[0].onload = function () {
    iosIframeWidthBug();
}
```

```javascript
/*修复ios iframe width bug*/
function iosIframeWidthBug(){
    //不是 iphone ipad就不执行了
    if (!navigator.userAgent.match(/iPad|iPhone/i)){
        return false;
    }else{
        //获取子iframe
        var iframebody = document.getElementById('iframe1').contentWindow.document.body;
        iframebody.style.width = document.body.clientWidth+'px';
    }
}
```
