---
layout: post
title: LocalStorage & SessionStorage
date: 2020-02-06 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - LocalStorage
---

# Storage
- 作为 Web Storage API 的接口，Storage 提供了访问特定域名下的会话存储（session storage）或本地存储（local storage）的功能，例如，可以添加、修改或删除存储的数据项。
- 如果你想要操作一个域名的会话存储（session storage），可以使用 window.sessionStorage如果想要操作一个域名的本地存储（local storage），可以使用  window.localStorage。
- sessionStorage和localStorage的用法是一样的，区别在于sessionStorage会在会话关闭也就是浏览器关闭时失效，而localStorage是将数据存储在本地，不受关闭浏览器影响，除非手动清除数据

```javascript
(function IIFE(){
    if(!window.localStorage){
        alert('your browser is not support localStorage!');
        return;
    }
    
    function getStorage(sname, defaultValue){
        //result = window.localStorage.sname
        //result = window.localStorage[sname]
        var result = window.localStorage.getItem(sname);
        return result || defaultValue;
    }
    
    function setStorage(sname, svalue){
        window.localStorage.setItem(sname, svalue);
    }
    
    function removeItem(sname){
        window.localStorage.removeItem(sname);
    }
    
    function getKey(keyIndex){
        return window.localStorage.key(keyIndex);
    }
    
    function getAllKeys(){
        var arr = [];
        for(var i=0;i<window.localStorage.length;i++){
            arr.push(window.localStorage.key(i));
        }
        return arr;
    }
    
    function clearStorage(){
        window.localStorage.clear();
    }
    
    function onStorageChange(event){
        console.log(event)
    }
    
    window.addEventListener('storage', onStorageChange);
    
    window.Util = {
        getStorage: getStorage,
        setStorage: setStorage,
        removeItem: removeItem,
        getKey: getKey,
        getAllKeys: getAllKeys,
        clearStorage: clearStorage,
    }
})();
```
