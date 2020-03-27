---
layout: post
title: Tool functions
date: 2020-03-28 15:00:00
categories:
    - JavaScript
tags: 
    - JavaScript
---

### Ajax 

```js
function createXHR() {
    var xhr;
    try {
        xhr = new ActiveXObject("Msxml2.XMLHTTP");
    } catch (e) {
        try {
            xhr = new ActiveObject("Microsoft.XMLHTTP");
        } catch (e) {
            xhr = false;
        }
    }

    if (!xhr && typeof XMLHttpRequest != 'undefined') {
        xhr = new XMLHttpRequest();
    }
    return xhr;
}

function ajax_send(val, callback) {
    xhr = createXHR();
    if (xhr) {
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4 && xhr.status == 200) {
                var returnValue = xhr.responseText;
                if (callback) {
                    callback(returnValue);
                }
            };
        }
        xhr.open("GET", val);
        xhr.send(null);
    }
}
```

### Cookie

```js
function GetCookie(c_name) {
    var c_start, c_end;
    if (document.cookie.length > 0) {
        c_start = document.cookie.indexOf(c_name + "=");
        if (c_start != -1) {
            c_start = c_start + c_name.length + 1;
            c_end = document.cookie.indexOf(";", c_start);
            if (c_end == -1) {
                c_end = document.cookie.length;
            }
            return unescape(document.cookie.substring(c_start, c_end));
        }
    }
    return "";
}

function SetCookie(name, value, cookiePath) {
    var expire;
    var isIE = !-[1, ]; //判断是否是ie核心浏览器  
    if (isIE) {
        expire = "; expires=At the end of the Session";
    } else {
        expire = "; expires=Session";
    }
    var path = "";
    if (cookiePath != null) {
         path = "; path=" + cookiePath;
    }
    document.cookie = name + "=" + escape(value) + expire + path;
}
```

### Minwidth, Resize


```js
function GetMinWidth() {
    return Math.ceil((window.screen.width - 160) * 0.55) - 6;
}

function Resize(Obj) {
    var MinWidth = GetMinWidth();
    if (window.document.body.offsetWidth > MinWidth) {
        Obj.autoWidth.style.width = "100%";
    } else {
        Obj.autoWidth.style.width = MinWidth;
    }
    return true;
}
```

### ini to JSON


```js
function ini2json(data) {
    var i;
    var len;
    var line;
    var regex = {
        section: /^\s*\[\s*([^\]]*)\s*\]\s*$/,
        param: /^\s*([\w\.\-\_]+)\s*=\s*(.*?)\s*$/,
        comment: /^\s*;.*$/
    };
    var value = {};
    var lines = data.split(/\r\n|\r|\n/);
    var section = null;
    len = lines.length;
    for (i = 0; i < len; i++) {
        line = lines[i];
        if (regex.comment.test(line)) {
            return;
        } else if (regex.param.test(line)) {
            var match = line.match(regex.param);
            if (section) {
                value[section][match[1]] = match[2];
            } else {
                value[match[1]] = match[2];
            }
        } else if (regex.section.test(line)) {
            var match = line.match(regex.section);
            value[match[1]] = {};
            section = match[1];
        } else if (line.length == 0 && section) {
            section = null;
        }
    }
    return value;
}
```

### RegExp


```js
function check_apha_number(str) {
    var reg = /^[0-9a-zA-Z]*$/g;
    return reg.test(str);
}

function checkip(ip) {
    var reg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/
    return reg.test(ip);
}

function checkport(port) {
    return (!isNaN(port) && port > 0 && port < 65536);
}
```