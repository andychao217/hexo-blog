---
layout: post
title: IndexedDB
date: 2020-02-19 12:00:00
categories:
    - Database
tags:
    - IndexedDB
    - 数据库
---

# IndexedDB
随着浏览器的功能不断增强，越来越多的网站开始考虑，将大量数据储存在客户端，这样可以减少从服务器获取数据，直接从本地获取数据。

现有的浏览器数据储存方案，都不适合储存大量数据：Cookie 的大小不超过 4KB，且每次请求都会发送回服务器；LocalStorage 在 2.5MB 到 10MB 之间（各家浏览器不同），而且不提供搜索功能，不能建立自定义的索引。所以，需要一种新的解决方案，这就是 IndexedDB 诞生的背景。

通俗地说，IndexedDB 就是浏览器提供的本地数据库，它可以被网页脚本创建和操作。IndexedDB 允许储存大量数据，提供查找接口，还能建立索引。这些都是 LocalStorage 所不具备的。就数据库类型而言，IndexedDB 不属于关系型数据库（不支持 SQL 查询语句），更接近 NoSQL 数据库。


```javascript
(function IIFE(){
    if(!window.indexedDB){
        alert('your browser is not support indexedDB!');
        return;
    }
    var request = window.indexedDB.open('person', 1);
    var db;
    request.onerror = function (event) {
        console.log('数据库打开报错');
    };
    request.onsuccess = function (event) {
        db = request.result;
        console.log('数据库打开成功');
    };
    request.onupgradeneeded = function(event) {
        console.log('onupgradeneeded...');
        db = event.target.result;
        var objectStore = db.createObjectStore('person', { keyPath: 'id' });
        objectStore.createIndex('name', 'name', { unique: false });
        objectStore.createIndex('email', 'email', { unique: true });
    }
    function add(obj) {
        var request = db.transaction(['person'], 'readwrite')
            .objectStore('person')
            .add(obj)
            //.add({ id: 1, name: 'ccy', age: 18, email: 'test@example.com' });

        request.onsuccess = function (event) {
            console.log('数据写入成功');
        };
        request.onerror = function (event) {
            console.log('数据写入失败');
        }
    }
    function read(index) {
        var transaction = db.transaction(['person']);
        var objectStore = transaction.objectStore('person');
        var request = objectStore.get(index);
        request.onerror = function(event) {
            console.log('事务失败');
        };

        request.onsuccess = function(event) {
            if (request.result) {
                console.log('Name: ' + request.result.name);
                console.log('Age: ' + request.result.age);
                console.log('Email: ' + request.result.email);
            } else {
                console.log('未获得数据记录');
            }
        };
    }
    function readAll() {
        var objectStore = db.transaction('person').objectStore('person');
        objectStore.openCursor().onsuccess = function (event) {
            var cursor = event.target.result;
            if (cursor) {
                console.log('Id: ' + cursor.key);
                console.log('Name: ' + cursor.value.name);
                console.log('Age: ' + cursor.value.age);
                console.log('Email: ' + cursor.value.email);
                cursor.continue();
            } else {
                console.log('没有更多数据了！');
            }
        };
    }
    function update(obj) {
        var request = db.transaction(['person'], 'readwrite')
            .objectStore('person')
            .put(obj)
            //.put({ id: 1, name: '李四', age: 35, email: 'lisi@example.com' });

        request.onsuccess = function (event) {
            console.log('数据更新成功');
        };

        request.onerror = function (event) {
            console.log('数据更新失败');
        }
    }
    function remove(index) {
        var request = db.transaction(['person'], 'readwrite')
            .objectStore('person')
            .delete(index);

        request.onsuccess = function (event) {
            console.log('数据删除成功');
        };
    }
    window.util = {
        add: add,
        read: read,
        readAll: readAll,
        update: update,
        remove: remove,
    }
})();
```
