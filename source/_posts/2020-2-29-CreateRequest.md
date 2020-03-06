---
layout: post
title: CreateRequest
date: 2020-02-29 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
---

```javascript

function createRequest(){
    try{
        request = new XMLHttpRequest();
    } catch (tryMS){
       try{
           request = new ActiveXObject("Msxm12.XMLHTTP");
       } catch (otherMS){
           try{
               request = new ActiveXObject("Microsoft.XMLHTTP");
           } catch (failed){
               request = null;
           }
       }
    } 
        
    return request;
}
```