---
layout: post
title: JS自动判断设备, 浏览器等
date: 2020-02-09 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 设备类型
---

```javascript
var ua = navigator.userAgent.toLowerCase();
```
## 区分移动设备

```javascript
if (/android|adr/gi.test(ua)) {
    // 安卓
     
}else if(/\(i[^;]+;( U;)? CPU.+Mac OS X/gi.test(ua)){
    //苹果
     
}else if(/iPad/gi.test(ua)){
    //ipad

}
```
## 区分浏览器
```javascript
    if(/msie/i.test(ua) && !/opera/.test(ua)){  
        alert("IE");  
        return ;  
    }else if(/firefox/i.test(ua)){  
        alert("Firefox");  
        return ;  
    }else if(/chrome/i.test(ua) && /webkit/i.test(ua) && /mozilla/i.test(ua)){  
        alert("Chrome");  
        return ;  
    }else if(/opera/i.test(ua)){  
        alert("Opera");  
        return ;  
    }else if(/iPad/i){ 
        alert("ipad"); 
        return ; 
    }else if(/webkit/i.test(ua) &&!(/chrome/i.test(ua) && /webkit/i.test(ua) && /mozilla/i.test(ua))){  
        alert("Safari");  
        return ;  
    }else{  
        alert("unKnow");  
    }  
```
## 区分软件内置浏览器，比如新浪微博、腾讯QQ（非QQ浏览器）和微信
```javascript

if(ua.match(/weibo/i) == "weibo"){  
    console.log(1);
}else if(ua.indexOf('qq/')!= -1){  
    console.log(2);
}else if(ua.match(/MicroMessenger/i)=="micromessenger"){  
var v_weixin = ua.split('micromessenger')[1];  
    v_weixin = v_weixin.substring(1,6);  
    v_weixin = v_weixin.split(' ')[0];  
if(v_weixin.split('.').length == 2){  
    v_weixin = v_weixin + '.0';  
}  
if(v_weixin < '6.0.2'){  
    console.log(3);
}else{  
    console.log(4);  
}  
}else{  
    console.log(0); 
}  
```