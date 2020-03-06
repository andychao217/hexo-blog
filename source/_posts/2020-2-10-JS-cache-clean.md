---
layout: post
title: JS清除浏览器缓存的几种方法
date: 2020-02-10 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 缓存
    - Cache
---

## 一、CSS和JS为什么带参数（形如.css?t=与.js?t=）怎样获取代码

css和js带参数（形如.css?t=与.js?t=） 
使用参数有两种可能： 
- 第一、脚本并不存在，而是服务端动态生成的，因此带了个版本号，以示区别。 即上面代码对于文件来说 等价于 但浏览器会认为他是 该文件的某个版本！ 
- 第二、客户端会缓存这些css或js文件，因此每次升级了js或css文件后，改变版本号，客户端浏览器就会重新下载新的js或css文件 ，刷性缓存的作用。 

第二种情况最多，也可能两种同时存在。 
版本号，可以是一个随机数，也可以是一个递增的值，大版本小版本的方式，或者根据脚本的生成时间书写，比如就是精确到了生成脚本的秒，而 2.3.3 就是大版本小版本的方式。

## 二、关于浏览器缓存

浏览器缓存，有时候我们需要他，因为他可以提高网站性能和浏览器速度，提高网站性能。但是有时候我们又不得不清除缓存，因为缓存可能误事，出现一些错误的数据。像股票类网站实时更新等，这样的网站是不要缓存的，像有的网站很少更新，有缓存还是比较好的。

今天主要介绍清除缓存的几种方法。

- meta方法

```HTML
<META HTTP-EQUIV="pragma" CONTENT="no-cache"> 
<META HTTP-EQUIV="Cache-Control" CONTENT="no-cache, must-revalidate"> 
<META HTTP-EQUIV="expires" CONTENT="0">
```

- 清理form表单的临时缓存 
1. 方式一：用ajax请求服务器最新文件，并加上请求头If-Modified-Since和Cache-Control,如下:

```javascript
$.ajax({
     url:'www.haorooms.com',
     dataType:'json',
     data:{},
     beforeSend :function(xmlHttp){ 
        xmlHttp.setRequestHeader("If-Modified-Since","0"); 
        xmlHttp.setRequestHeader("Cache-Control","no-cache");
     },
     success:function(response){
         //操作
     }
     async:false
  });
```

2. 方法二，直接用cache:false,

```javascript
$.ajax({
     url:'www.haorooms.com',
     dataType:'json',
     data:{},
     cache:false, 
     ifModified :true ,

     success:function(response){
         //操作
     }
     async:false
  });
```

3. 方法三：用随机数，随机数也是避免缓存的一种很不错的方法！

```javascript
URL 参数后加上 "?ran=" + Math.random(); //当然这里参数 ran可以任意取了

<script> 
document.write("<s"+"cript type='text/javascript' src='/js/test.js?"+Math.random()+"'></scr"+"ipt>"); 
</script>

其他的类似，只需在地址后加上+Math.random() 
注意：因为Math.random() 只能在Javascript 下起作用，故只能通过Javascript的调用才可以
```
 
4. 方法四：
- 用随机时间，和随机数一样。

```javascript
在 URL 参数后加上 "?timestamp=" + new Date().getTime();
```
- 用PHP后端清理

```javascript
在服务端加 header("Cache-Control: no-cache, must-revalidate");等等(如php中)
```

5. 方法五：
```javascript
window.location.replace("WebForm1.aspx");   
参数就是你要覆盖的页面，replace的原理就是用当前页面替换掉replace参数指定的页面。   
这样可以防止用户点击back键。使用的是javascript脚本，举例如下： 

a.html 
以下是引用片段： 
```HTML
<html> 
     <head> 
         <title>a</title>      
         <script language="javascript"> 
             function jump(){ 
                 window.location.replace("b.html"); 
             } 
         </script> 
     </head> 
     <body> 
        <a href="javascript:jump()">b</a> 
    </body> 
</html>  
```
b.html 
以下是引用片段： 

```HTML
<html> 
     <head> 
         <title>b</title>      
         <script language="javascript"> 
             function jump(){ 
                 window.location.replace("a.html"); 
             } 
         </script> 
     </head> 
     <body> 
        <a href="javascript:jump()">a</a> 
    </body> 
</html>
```
