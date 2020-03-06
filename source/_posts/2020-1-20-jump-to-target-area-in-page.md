---
layout: post
title: 页面跳转至指定位置
date: 2020-01-20 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - Url
---

###### 方法主要利用scrolltop值做运动， 用于到达用户指定的位置（如返回顶部把参数target设置为0即可），处理了多种情况如 scrolltop > 目标值 向上运动 ，等4种情况  ， 代码及用法贴上：
```javascript
goTo = function(target){
    var scrollT = document.body.scrollTop|| document.documentElement.scrollTop
    if (scrollT >target) {
        var timer = setInterval(function () {
            var scrollT = document.body.scrollTop|| document.documentElement.scrollTop
            var step = Math.floor(-scrollT/6);
            document.documentElement.scrollTop = document.body.scrollTop = step + scrollT;
            if (scrollT <= target) {
                document.body.scrollTop = document.documentElement.scrollTop = target;
                clearTimeout(timer);
            }
        }, 20)
    } else if (scrollT == 0) {
        var timer = setInterval(function () {
            var scrollT = document.body.scrollTop|| document.documentElement.scrollTop
            var step = Math.floor(300/3*0.7);
            document.documentElement.scrollTop = document.body.scrollTop = step + scrollT;
            console.log(scrollT)
            if (scrollT >= target) {
                document.body.scrollTop = document.documentElement.scrollTop = target;
                clearTimeout(timer);
            }
        },20)
    } else if (scrollT < target) {
        var timer = setInterval(function () {
            var scrollT = document.body.scrollTop|| document.documentElement.scrollTop
            var step = Math.floor(scrollT/6);
            document.documentElement.scrollTop = document.body.scrollTop = step + scrollT;
            if (scrollT >= target) {
                document.body.scrollTop = document.documentElement.scrollTop = target;
                clearTimeout(timer);
            }
        },20)
    } else if (target == scrollT) {
        return false;
    }
}
```
###### 用法 function(target) / / 目前唯一target（目标距离number）
```javascript
on(goPs,'click',function(){ goTo(2450) }); //运动到scrolltop值为2450地位置，下面也一样， 运动到指定的位置
on(goTop,'click',function(){ buffer.goTo(0) })
```
