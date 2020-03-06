---
layout: post
title: Bind()辅助函数
date: 2020-03-02 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
---

```javascript
function foo(somethine){
    console.log(this.a, something);
    return this.a + something;
}

//bind辅助函数
function bind(fn, obj){
    return function(){
        return fn.apply(obj, arguments);
    }
}

var obj = {
    a:2
};

var bar = bind(foo, obj);
var b = bar(3); //2+3
console.log(b); //5
```
