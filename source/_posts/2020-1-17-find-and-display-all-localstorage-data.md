---
layout: post
title: 查找并显示所有LocalStorage数据
date: 2020-01-17 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - LocalStorage
---

```javascript
function findAllLocalStorageItems(){
    var itemList = document.getElementById('itemlist');
    
    itemList.innerHTML = '';
    
    for(var i = 0; i<localStorage.length; i++){
        var key = localStorage.key(i);
        
        var item = localStorage.getItem(key);
        
        var newItem = document.createElement('li');
        newItem.innerHTML = key + ": " + item;
        itemList.appendChild(newItem);
    }
}
```
